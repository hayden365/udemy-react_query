# Lazy Days Spa Client

## 규모가 큰 앱에서 React Query

- 큰 앱에서 쿼리에서 데이터를 새로 고침하게끔 제어할 수 있어야 한다.
- 인증과정에서 리액트쿼리

## React Query ESLint 플러그인

### @tanstack/query/prefer-query-object-syntax

현재 useQuery에 인수를 제공하는 방법은 두 가지입니다:

인수 여러 개:

```
useQuery(queryKey, queryFn, { onSuccess,});
```

객체 인수 한 개:

```
useQuery({ queryKey, queryFn, onSuccess,});
```

## TypeScript

타입스크립트 에러를 무시하는 기능

- @ts-nocheck : 파일의 가장위에서 작성
- @ts-ignore : 특정 줄을 위에 작

## 커스텀 훅을 이용한 데이터 액세스

앱의 크기가 커지고, 다수의 컴포넌트에서 데이터를 액세스해야하는 경우 useQuery 호출을 재작성할 필요가 없도록 커스텀 훅을 만들어서 사용하자.
