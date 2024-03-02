# PrimeReact APOLLO Documentation

## 시작하기 - Getting Started

Apollo는 인기 있는 [Next.js](https://nextjs.org/) 프레임워크를 기반으로 한 React용 애플리케이션 템플릿으로, 새로운 [App Router](https://nextjs.org/docs/app)를 사용합니다. 현재 버전은 Next v13, React v18, Typescript와 PrimeReact v10입니다.

```bash
"npm install" 또는 "yarn"
```

다음 단계는 start 스크립트를 사용하여 애플리케이션을 실행하고 **http://localhost:3000/** 로 이동하여 애플리케이션을 확인하는 것입니다. 이것으로 끝이며, 이제 Apollo 템플릿을 사용하여 애플리케이션 개발을 시작할 수 있습니다.

```bash
"npm run dev" 또는 "yarn dev"
```

---

## 의존성 - Dependencies

Apollo의 의존성은 아래에 나열되어 있으며 `package.json`에서 정의해야 합니다.

```
"primereact": "^10.2.1",                    //필수: PrimeReact 컴포넌트
"primeicons": "^6.0.1",                     //필수: 아이콘
```

---

## 구조 - Structure

Apollo는 몇 개의 폴더로 구성되어 있으며, 데모와 코어가 분리되어 있어 애플리케이션에 필요하지 않은 것을 쉽게 제거할 수 있습니다.

app 폴더 아래에는 두 개의 [라우트 그룹](https://nextjs.org/docs/app/building-your-application/routing/route-groups)이 있습니다; (main)은 메인 대시보드 레이아웃에 있는 페이지를 나타내고 (full-page)는 랜딩 페이지나 로그인 페이지 같은 전체 페이지 콘텐츠를 그룹화합니다.

- **`layout:`** 메인 레이아웃 파일, 반드시 존재해야 함
- **`demo/:`** 데모 관련 유틸리티와 헬퍼들을 포함
- **`app/:`** 데모 페이지
- **`public/demo:`** 데모에서 사용된 에셋
- **`public/layout:`** 레이아웃에서 사용된 에셋 예: 로고
- **`styles/demo:`** 데모에서만 사용된 CSS 파일
- **`styles/layout:`** 코어 레이아웃의 SCSS 파일

---

## 라우트 그룹 - Route Groups

Root Layout은 애플리케이션의 메인이며 `app/layout.tsx` 파일에서 정의됩니다. 스타일 임포트와 레이아웃 컨텍스트 제공자를 포함하고 있습니다.

`app/layout.tsx`

```tsx
"use client"
import { LayoutProvider } from "../layout/context/layoutcontext";
import { PrimeReactProvider } from "primereact/api";

import '../styles/layout/layout.scss';
...
import 'primereact/resources/primereact.css';
import '../styles/demo/Demos.scss';

interface RootLayoutProps {
  children: React.ReactNode;
}

export default function RootLayout({ children }: RootLayoutProps) {
  return (
    <html lang="en" suppressHydrationWarning>
      <head>
        <link
          id="theme-link"
          href={`/theme/theme-light/blue/theme.css`}
          rel="stylesheet"
        ></link>
      </head>
      <body>
        <PrimeReactProvider>
            <LayoutProvider>{children}</LayoutProvider>
        </PrimeReactProvider>
      </body>
    </html>
  );
}
```

메인 대시보드 레이아웃을 사용하는 페이지들은 `app/(main)/` 폴더 아래에 정의되어야 합니다. 이러한 페이지들은 `app/(main)/layout.tsx` 를 루트 레이아웃으로 사용합니다.

`app/(main)/layout.tsx`

```tsx
import { Metadata } from "next";
import Layout from '../../layout/layout';
interface MainLayoutProps {
    children: React.ReactNode;
}

export const metadata: Metadata = {
    title: "PrimeReact Apollo",
    ...
  };

export default function MainLayout({ children }: MainLayoutProps) {
    return <Layout>{children}</Layout>;
}
```

전체 페이지 콘텐츠 페이지들은 대시보드 레이아웃 바깥에 있기 때문에 `app/(full-page)/` 폴더 아래에 정의되어야 합니다. 이러한 페이지들은 `app/(full-page)/layout.tsx`를 루트 레이아웃으로 사용합니다.

`app/(full-page)/layout.tsx`

```tsx
import { Metadata } from "next";
import AppConfig from '../../layout/AppConfig';
import React from 'react';

interface FullPageLayoutProps {
    children: React.ReactNode;
}

export the metadata: Metadata = {
    title: "PrimeReact Apollo",
    ...
};

export default function FullPageLayout({ children }: FullPageLayoutProps) {
    return (
        <React.Fragment>
            {children}
            <AppConfig minimal />
        </React.Fragment>
    );
}

```

---

## 기본 구성 - Default Configuration

레이아웃의 초기 구성은 `layout/context/layoutcontext.js` 파일에서 정의할 수 있으며, 이 단계는 기본값을 사용자 정의할 때만 필요합니다.

`layout/context/layoutcontext.js`

```js
"use client"
import React, { useState } from 'react';
import Head from 'next/head';

export const LayoutContext = React.createContext();

export the LayoutProvider = (props) => {
    const [breadcrumbs, setBreadcrumbs] = useState({});
    const [layoutConfig, setLayoutConfig] = useState({
        ripple: true,                          //리플 켜기/끄기 토글
        inputStyle: 'outlined',                 //입력 요소의 기본 스타일
        menuMode: 'static',                     //메뉴의 레이아웃 모드, 유효한 값은 "static", "overlay", "slim", "horizontal", "reveal", "drawer"
        menuTheme: 'colorScheme',               //메뉴의 테마, 유효한 값은 "colorScheme", "primaryColor", "transparent"
        colorScheme: 'light',                   //템플릿의 색상 체계, 유효한 값은 "light", "dim", "dark"
        theme: 'indigo',                        //PrimeReact의 기본 컴포넌트 테마
        menuTheme: "colorScheme",               //메뉴의 테마, 유효한 값은 "colorScheme", "primaryColor", "transparent"
        scale: 14                               //전체 애플리케이션을 스케일링하는 본문 폰트 크기의 크기
    });
}
```

---

## 메뉴 - Menu

메뉴는 `layout/AppMenu.js` 파일에 정의된 별도의 컴포넌트입니다. 메뉴 아이템을 정의하려면 이 파일로 이동하여 자신의 모델을 중첩 구조로 정의하십시오.

`layout/AppMenu.js`

```js
import React from 'react';
import AppMenuitem from './AppMenuitem';
import { MenuProvider } from './context/menucontext';

const AppMenu = () => {
    the model = [
        {
            label: 'Dashboards',
            icon: 'pi pi-home',
            items: [
                {
                    label: 'E-Commerce',
                    icon: 'pi pi-fw pi-home',
                    to: '/'
                },
                {
                    label: 'Banking',
                    icon: 'pi pi-fw pi-image',
                    to: '/dashboard-banking'
                }
            ]
        },
            //...
        ];
}
```

---

## 브레드크럼 - Breadcrumb

상단바 섹션의 Breadcrumb 컴포넌트는 동적이며, 렌더링된 메뉴 아이템에서 현재 라우트 정보를 생성합니다.

---

# 테마 - Theme

Apollo는 기본적으로 24개의 PrimeReact 테마를 제공합니다. 테마를 설정하는 것은 `public/theme/` 폴더 내에 있는 테마의 CSS를 번들에 포함시켜 간단히 수행할 수 있습니다.

사용자 정의 테마는 다음 단계를 따라 개발할 수 있습니다.

- `“mytheme“`과 같은 사용자 정의 테마 이름을 선택합니다.
- `theme/theme-light/` 폴더 아래에 `“mytheme“` 라는

폴더를 생성합니다.

- `“mytheme“` 폴더 안에 `theme.scss` 파일을 생성합니다.
- 파일에서 아래에 나열된 변수를 정의하고 의존성을 임포트합니다.
- 애플리케이션에 `theme.scss` 를 포함시킵니다.

테마를 생성하기 위해 필요한 변수들입니다.

```css
$primaryColor: #3b82f6 !default;
$primaryLightColor: #bfdbfe !default;
$primaryDarkColor: #2563eb !default;
$primaryDarkerColor: #1d4ed8 !default;
$primaryTextColor: #ffffff !default;
$primary500: #3b82f6 !default;

$highlightBg: #eff6ff !default;
$highlightTextColor: $primaryDarkerColor !default;

@import "../_variables";
@import "../../theme-base/_components";
@import "../_extensions";
```

---

## 테마 스위처

동적 테마는 템플릿에 내장되어 있으며, 링크 태그를 통해 테마를 포함하여 구현되며, 테마를 동적으로 전환하기 위한 구성 요소와 함께 번들링하지 않습니다. 테마를 동적으로 전환하려면 css로 컴파일해야 합니다. css를 컴파일하는 예제 sass 명령은 다음과 같습니다;

```bash
sass --update public/theme/mytheme/theme.scss:public/theme/mytheme/theme.css
```

\*참고: 위의 sass 명령은 Dart Sass에서 지원됩니다. Ruby Sass 대신 Dart Sass를 사용하세요.

---

## 마이그레이션

**CHANGELOG.md** 파일에 중요한 변경 사항이 포함되어 있으며, 배포의 루트 폴더에 업데이트 지침과 함께 포함되어 있습니다. 마이그레이션 프로세스는 주로 `layout` 폴더와 `styles/layout` 폴더를 업데이트하는 것을 요구합니다.
