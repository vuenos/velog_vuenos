# Next.js Route Groups

## 화면의 성격에 맞는 레이아웃을 구성해야 할 때
화면의 레이아웃을 구성할 떄 기능 또는 화면성격에 맞게 레이아웃을 다르게 해야할 경우가 있다.
- 로그인 화면: 공통영역(GNB, Navigation 등) 없이 로그인 폼만 배치
- 컨텐츠 페이지: 공통영역(GNB, Navigation, Breadcrumbs, Footer 등) 과 컨텐츠 내용으로 구성
- 위의 내용과 다른 형태의 레이아웃 페이지

### react-router-dom 의 useRoutes 경우는
- useRoutes 의 element 에 ```<LayoutMainProvider />``` 와 같은 레이아웃 컴포넌트를 만들어서
- children 안에 화면별 ```path``` 와 ```element``` 객체를 추가하여 레이아웃을 구성할 수 있다.
```javascript
{
    path: "/", 
    element: <LayoutMainProvider />, 
    children: [
        { path: "/dashboard", element: <Dashboard /> },
    ]
    
}
```

### Next.js 15 의 Route Groups 경우는 (/app route)
- [공식문서](https://nextjs.org/docs/app/building-your-application/routing/route-groups)
- /app/layout.ts 는 모든 화면의 공통 레이아웃으로 설정된다. (root layout)
- /app/login/page.tsx: /login
- /app/shop/page.tsx: /shop

### (segment name) 디렉토리 생성
예를 들어 /auth 와 /shop 페이지의 화면 레이아웃 다르게 해야할 경우
- /app/(auth)/login
- /app/(shop)/shop
- 디렉토리를 (auth) 와 같이 네이밍하여 만들면 실제 route 주소에는 반영되지 않는다. (e.g. /app/(auth)/login/page.tsx 는 /login 이라는 route 로 설정됨)

### layout.tsx 생성
- /app/(auth)/layout.tsx 를 생성하면
- /app/(auth)/login/page.tsx 와 같은 하위 디렉토리의 화면들에 일괄 적용된다.
- 이렇게 기능별로 레이아웃을 설정할 수 있고 시각적으로 리소스의 정의가 가능해 진다.

![공식사이트 참고이미지](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Fdark%2Froute-group-opt-in-layouts.png&w=1920&q=75)