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

## 중앙화된 로딩

useIsFetching(현재 가져오고 있는 데이터가 있는지 알려준다.)을 사용해서 만듭니다.

## 발생한 에러를 Toast에 전달

useQuery call의 onError를 사용합니다.

```
export function useTreatments(): Treatment[] {
  const toast = useCustomToast();

  const fallback = [];
  const { data = fallback } = useQuery(queryKeys.treatments, getTreatments, {
    onError: (error) => {
      const title =
        error instanceof Error
          ? error.message
          : 'error connecting to the server';
      toast({ title, status: 'error' });
    },
  });

  return data;
}
```

### useError훅을 사용하지 않는 이유

정수 이상의 값이 반환되어야한다. 각 오류에 대한 문자열이 필요한데 각각의 오류를 컨트롤하기에 useError는 적절하지 않습니다.

### QueryClient를 사용한 에러 핸들링

- 일반적으로 QuuryClient는 쿼리나 변이(Mutation)에 대해 기본값을 가질 수 있다.
- useQuery 마다 오류 핸들러를 추가할 필요가 없어진다.

```
import { createStandaloneToast } from '@chakra-ui/react';
import { QueryClient } from 'react-query';
import { theme } from '../theme';
const toast = createStandaloneToast({ theme });

function queryErrorHandler(error: unknown): void {
// error is type unknown because in js, anything can be an error (e.g. throw(5))
  const title = error instanceof Error ? error.message : 'error connecting to  server';

// prevent duplicate toasts
  toast.closeAll();
  toast({ title, status: 'error', variant: 'subtle', isClosable: true });
}

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      onError: queryErrorHandler,
      },
    },
});
```

### useErrorBoundary 에러 핸들링

각 useQuery및 useMutaion에 옵션으로 추가하거나 QueryClient를 생성할 때 default로 사용할 수 있습니다. useErrorBoundary옵션을 true로 설정하면 React Query 내에서 에러를 처리하는 대신 가장 가까운 에러 경계로 에러를 전파합니다.

## 전역 오류 핸들러를 지정하는 더 나은 방법

앞서 소개한 방법에서 react-query는 에러발생시 3번을 시도한다는 특성상 매번 시도할떄마다 toast의 중복호출상황을 방지하기 위해서 toast.closeAll()을 사용했었는데요.  
이 방법 대신 queryCache옵션을 사용하면 재시도로 인한 토스트를 방지할 수 있게 됩니다.

```
import { createStandaloneToast } from '@chakra-ui/react';
import { QueryCache, QueryClient } from 'react-query';

import { theme } from '../theme';

const toast = createStandaloneToast({ theme });

function queryErrorHandler(error: unknown): void {
  // error is type unknown because in js, anything can be an error (e.g. throw(5))
  const title =
    error instanceof Error ? error.message : 'error connecting to server';

  ///////////////////////////////
  // NOTE: no toast.closeAll() //
  ///////////////////////////////

  toast({ title, status: 'error', variant: 'subtle', isClosable: true });
}

export const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: queryErrorHandler,
  }),
});
```
