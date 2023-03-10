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
