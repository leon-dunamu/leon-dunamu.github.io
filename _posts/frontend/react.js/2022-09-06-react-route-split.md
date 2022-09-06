---
layout: post
subtitle: "React.lazy를 활용한 초기 로딩 속도 개선"
title: "리액트 Route를 Code Splitting으로 개선하기"
author: "Seog"
header-style: text
tags:
  - Frontend
  - React.js
---

프로젝트를 진행하며 로그 관리, 팝업 관리 등의 여러 데이터를 어드민 계정으로 한눈에 보기 위해 관리자 페이지를 만들어 왔었다. 처음엔 간단한 React CRA 웹페이지였지만 요구사항이 늘어가며 점점 페이지 수가 많아졌고, 웹팩을 이용해 하나의 번들로 빌드해주기에 초기 로딩 속도가 점점 느려져 `DOMContentLoaded` 시간이 1.1초대 이상을 유지하게 되었다.

물론 여러가지 인증 정보를 거친 이후에 웹 페이지를 모두 렌더시키다보니 타 웹페이지에 비해서 느린 감이 있었지만, 초기 번들파일이 `1.9M`로 늘어나며 초기로딩 속도가 더욱 늦어지게 되었다.

문제는 `Route`를 등록할 시, 미리 모든 페이지들을 불러오기에 현재 보여지지 않는 페이지들도 미리 불려져서 초기 번들 파일이 커지게 되었던 것이었다. 이를 파악한 후 `lazy import`를 활용하여 현재 페이지만을 동적으로 불러오도록 진행해보았다.

## 1. 프로젝트의 상황

페이지가 수십페이지를 넘어가게 되면서, 기존에 분리해놓았던 Route전용 파일도 넘쳐나게 되었다.

```js
// src/Libs/Routes.ts
import LogsPage from "Pages/Logs";
import LogsDetailPage from "Pages/Logs/Detail";

/**
 * 수많은 페이지 중간 생략
 **/

import UsersPage from "Pages/Users";
import UsersCreatePage from "Pages/Users/Create";
import UsersModifyPage from "Pages/Users/Modify";

export const PrivateRoutes = {
  logs: {
    path: "/logs",
    component: LogsPage,
  },
  logsDetail: {
    path: "/logs/:id",
    component: LogsDetailPage,
  },

  /**
   * 수많은 페이지 중간 생략
   **/

  users: {
    path: "/users",
    component: UsersPage,
  },
  usersCreate: {
    path: "/users/create",
    component: UsersCreatePage,
  },
  usersModify: {
    path: "/users/:id",
    component: UsersModifyPage,
  },
};
```

```jsx
// src/App.ts

import React from "react";
import { Route, Switch, BrowserRouter } from "react-router-dom";
import { PrivateRoutes, PublicRoutes } from "Libs/Routes";
import { useToken } from "Hooks/useToken";

const App: React.FC = () => {
  const { token } = useToken();

  return token ? (
    <BrowserRouter basename={partner.uniqueId.toLowerCase()}>
      <Switch>
        {/* 초기에 모든 페이지를 불러와 Route로 등록 */}
        {token ? (
          Object.values(PrivateRoutes).map((privateRoute) => (
            <Route
              key={privateRoute.path}
              path={privateRoute.path}
              component={privateRoute.component}
              exact
            />
          ))
        ) : (
          <Route
            path={PublicRoutes.logIn.path}
            component={PublicRoutes.logIn.component}
          />
        )}
      </Switch>
    </BrowserRouter>
  ) : (
    <div>loading ...</div>
  );
};

export default App;
```

위와 같이 Route.ts에서 초기에 모든 페이지들을 import 해오게 되면서 초기 번들 파일이 매우 무겁게 되었다.

우선 Pages 폴더 구조는 다음과 같이 되어있었다.

```txt
src
 |- Pages
 |   |- Logs
 |   |   |- index.tsx
 |   |   |- Detail.tsx
 |   |
 | 중간 폴더 생략
 |   |
 |   |- Users
 |       |- index.tsx
 |       |- Create.tsx
 |       |- Modify.tsx
```

이렇게 경로별로 매칭하도록 폴더 및 파일 구조를 맞춰줘야한다.

> 예를들어 path가 `/users/create`인 페이지는 폴더 및 파일 구조를 `src/Pages/Users/Create`로,,

다만 이미 페이지들이 모두 첫 글자가 대문자로 제작되어 있었다.
이를 모두 소문자로 바꾸기에는 컨벤션에 따라 다른 폴더 및 파일들도 소문자로 바꿔주는게 맞을 것 같다고 느껴 간단히 import시에만 path의 각 첫 글자를 대문자로 변환하게 진행하였다.

## 2. 비동기 import 훅 제작

비동기로 import해오다 보니 로딩이 완료된 시점 이후의 처리가 필요해 보여 각 Route에 대해 hooks로 처리해주도록 진행하였다.

```js
// src/Libs/Path.ts

export function capitalizePath(path: string) {
  return path
    .split("/")
    .map((_path) => capitalizeFirstLetter(_path))
    .join("/");
}

function capitalizeFirstLetter(string: string) {
  return string.charAt(0).toUpperCase() + string.slice(1);
}
```

```jsx
// src/Hooks/useRouteComponent.ts

import { capitalizePath } from "Libs/Path";
import React, { useState, useEffect } from "react";

const pageImport = (path: string) => import(`../Pages${path}`); // (1)

const useRouteComponent = (path: string) => {
  const [component, setComponent] = useState(null);

  useEffect(() => {
    const importAsyncPage = async () => {
      const { default: Page } = await pageImport(capitalizePath(path));

      setComponent(() => Page);
    };

    importAsyncPage();
  }, [path]);

  return {
    component,
  };
};

export default useRouteComponent;
```

위의 (1)부분을 처음에는

```js
const pageImport = (path: string) => import(`Pages${path}`);
```

위 (1)와 같이 절대경로로 진행해보았으나 `Module not found`문구가 떠서 상대경로로 우선 진행했다.

위처럼 `importAsyncPage`을 통해 페이지를 비동기로 불러와서 해당 path에 해당하는 페이지를 반환해준다.

위를 사용하면 Route에서는 아래와 같이 적용할 수 있다.

## 3. 훅 적용

```jsx
import React from "react";
import { Route, Switch, BrowserRouter } from "react-router-dom";
import { PrivateRoutes, PublicRoutes } from "Libs/Routes";
import { useToken } from "Hooks/useToken";

export interface RouteType {
  path: string
  component?: React.FC<RouteComponentProps<{}, StaticContext, unknown>>
}

interface PageRouteProps {
  path: string
}

const PageRoute = ({ path }: PageRouteProps) => {
  const { component } = useRouteComponent(path)

  return (
    <Route path={path}>{component ? createElement(component) : null}</Route>
  )
}

const App: React.FC = () => {
  const { token } = useToken();

  const pages = Object.values(PrivateRoutes)

  let modifyPages: RouteType[] = []
  let otherPages: RouteType[] = []
  pages.forEach(page =>
    page.path.includes(':id') ? modifyPages.push(page) : otherPages.push(page) // <--- (2)
  )

  return token ? (
    <BrowserRouter basename={partner.uniqueId.toLowerCase()}>
      <Switch>
        {/* 초기에 모든 페이지를 불러와 Route로 등록 */}
        {token ? (
          <>
            {otherPages.map(route => {
              const isActive = matchPath(location.pathname, {
                path: route.path,
              })?.isExact

              return isActive ? (
                <PageRoute key={route.path} path={route.path} />
              ) : null
            })}
            {modifyPages.map(route => {
              if (!route.roles.includes(role)) return null

              return (
                <Route
                  key={route.path}
                  path={route.path}
                  component={route.component}
                  exact
                />
              )
            })}
              </>
        ) : (
          <Route
            path={PublicRoutes.logIn.path}
            component={PublicRoutes.logIn.component}
          />
        )}
      </Switch>
    </BrowserRouter>
  ) : (
    <div>loading ...</div>
  );
};

export default App;
```

위와 같이 최종적으로 적용해볼 수 있다. 다만 상세 조회 페이지나, 수정 페이지의 경우 path가 `:id`와 같은 동적 라우팅 형식으로 되어있다. 해당 path에 맞게 페이지 컴포넌트 파일 명을 `:id`로 해주는 게 맞는 것 같아 보이지만, 컴포넌트의 파일 이름은 이와 같은 형식이 될 수 없었다.

따라서 위의 (2)처럼 path에 `:id`가 포함되어있는 Route는 따로 필터처리 해서 렌더링 시키도록 진행하였다.

이렇게 동적으로 import해오니 약 400ms 만큼의 초기 로딩속도롤 줄이게 되었다.
