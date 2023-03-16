# udemy-REACT-QUERY

Code to support the Udemy course [React Query: Server State Management in React](https://www.udemy.com/course/learn-react-query/?couponCode=REACT-QUERY-GITHUB)

## 목차

[#1 Blog-em Ipsum](#blog-em-ipsum)  
[#2 Infinite Swapi](#infinite-swapi)  
[#3 Lazy Days Spa Client](#lazy-days-spa-client)  
[#4 Pre-fetching and Pagination](#pre-fetching-and-pagination)

# Blog-em Ipsum

간단한 블로그 앱

- Fetching Data
- Loading/ error states
- React Query dev tools
- Pagination
- Prefetching
- Mutations

### useQuery

특정 쿼리의 상태를 사용자에게 알려줄 때 사용합니다.

### isFetching , isLoading

isFetching은 isLoading을 포함하는 개념입니다.  
isFetching에서는 캐시된 데이터의 쿼리가 호출 중일 수 있습니다.
isLoading은 쿼리가 만들어지지도 않았을 수 있어서 캐시된 데이터도 없습니다.  
react query에서 error로 명시하기까지 기본 세번은 시도를 한 후 error를 정의합니다.

- isFetching : async쿼리 함수가 해결되지 않았을 때, 참에 해당합니다. 아직 데이터를 가져오는 중임을 나타냅니다.
- isLoading : isFetching이 참이면서 쿼리에 대해 캐시된 데이터 없는 상태를 뜻합니다. isLoading이 참이면 isFetching은 항상 참이 됩니다.

### staleTime, cacheTime

staleTime은 만료시간을 정의하고 만료되었다면 데이터를 리페칭하게 됩니다. 기본값을 0입니다. 만료된 데이터 외에도 항상 서버에서 가져오게 하도록 하는 설정이 가능합니다.  
cacheTime은 데이터의 유효시간이며, 기본값은 5분입니다. useQuery가 활성화된 후 부터 경과한 시간입니다. 서비스특성상 만료된 데이터가 위험한 경우 cacheTime을 0으로 설정하면 데이터가 만료된 경우를 방지할 수 있습니다.

staleTime : 윈도우가 다시 포커스될 때 같은 특정 트리거에서 쿼리 데이터를 다시 가져올지를 결정함. 즉, 데이터가 사용 가능한 상태로 유지되는 시간이다.

cacheTime : 데이터가 비활성화된 이후 남아 있는 시간을 말합니다. 캐시된 데이터는 쿼리를 다시 실행했을 때 사용됩니다.

### pagination, pre-fetching

useQueryClient의 prefetchQuery를 이용해서 프리페칭 페이지네이션을 구현했습니다.
페이지가 바뀔 때마다 해당 페이지의 다음페이지를 프리페칭하기 때문에 useEffect를 이용해 관리했습니다.

### useMutation

useQuery와 비슷하지만 다음과 같은 차이점이 있습니다.

- mutation 함수를 return 합니다.
- 쿼리키를 필요로 하지 않습니다.
- 기본적으로 재시도는 없습니다.(useQuery는 3번재시도하는 것에 비해)

# Infinite SWAPI

무한스크롤 구현

## useInfiniteQuery vs useQuery

useQuery에서는 데이터는 단순히 쿼리 함수에서 반환되는 데이터
useInfiniteQuery에서 데이터객체는 pages, pageParams 두개가 있다.  
pages는 데이터 페이지의 객체의 배열입니다. pageParams는 각 페이지의 매개변수가 기록되어있습니다.

```
useInfiniteQuery("sw-people", ({ pageParam = defaultUrl}) => fetchUrl(pageParam))
```

### useInfiniteQuery options

- getNextPageParam : (lastPage, allPages)
  다음 페이지에 대한 형식을 정의한다. pageParam을 업데이트해준다.

- fetchNextPage : 더 많은 데이터를 요청할 때 호출하는 함수
- hasNextPage : getNextPageParam 함수의 반환 값을 기반으로 한다. 이 프로퍼티를 useInfiniteQuery에 전달해서 마지막 쿼리의 데이터를 어떻게 사용할 지 지시함.  
  undefined인 경우 더 이상 데이터가 없다는 거고 useInfiniteQuery에서 반환 객체와 함께 반환된 경우 hasNextPage는 거짓이 된다.
- isFetchingNextPage : 로딩 스피너를 보여줄 수 있다.

### Infinite Scroller

react-infinite-scroller는 useInfiniteQuery와 함께 잘 작동합니다.

- loadMore = {fetchNextPage}
- hasMore = {hasNextPage}
  무한 스크롤 컴포넌트는 페이지의 끝에 도달했음을 인식하고 fetchNextPage를 불러오는 기능입니다.
  useInfiniteQuery컴포넌트에서 나온 객체를 이용합니다. 배열인 페이지 프로퍼티를 이용해서 그 페이지 배열의 맵을 만드렁 데이터를 표시할 수 있게 해줍니다.
  ```
   <InfiniteScroll loadMore={fetchNextPage} hasMore={hasNextPage} />;
  ```

### 양방향 스크롤

시작점 이후뿐 아니라 이전의 데이터도 가져와야 한다.

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

# Pre-fetching and Pagination

|                 | where to use?         | data from? | added to cache? |
| --------------- | --------------------- | ---------- | --------------- |
| prefetchQuery   | method to queryClient | server     | yes             |
| setQueryData    | method to queryCLient | client     | yes             |
| placeholderData | option to useQuery    | client     | no              |
| initialData     | option to useQuery    | client     | yes             |

## Prefetch

무한스크롤을 구현하면서 사용자가 현재 페이지를 보고있는 동안 다음페이지 데이터를 미리 가져오는 prefetch를 구현했습니다.  
prefetch를 사용하는 또다른 예시는 다음과 같습니다.  
홈페이지 로드 중 높은 비율이 treatments탭으로 이어진다는 가정 하에 홈페이지에서 treatments탭의 내용을 prefetch하는 것입니다.  
이떄, prefetch한 데이터의 cacheTime이 5분이라면, 5분안에 사용자가 treatments탭을 클릭했을 때, 사용자는 prefetch데이터를 바로 확인할 수 있게 됩니다.  
여기서 중요한점은 불러온 prefetch 데이터는 다시 한번 더 로드 됩니다.(최신의 데이터를 위해서)  
만약 캐시타임(5분)안에 treatments탭으로 이동하지 않았을 경우 해당 데이터는 가비지 컬렉션으로 수집됩니다.  
기존의 useQuery역시 페칭과 리페칭 등의 작업이 필요한 쿼리를 생성하지만 prefetchQuery는 일회성이라는 차이점이 있습니다.  
따라서 prefetchQuery는 Home 컴포넌트에서 실행되어야 합니다.  
참고로, Home컴포넌트가 그다지 동적이지않아서, 리랜더가 많지 않고, treatments탭의 데이터 또한 동적이지 않아서 가능한 전략이기도 합니다.
만약 걱정된다면, staleTime과 cacheTime을 관리해주는 옵션을 추가해주면 됩니다. 그러면 모든 트리거마다 리페칭되지 않도록 통제할 수 있습니다.

```
export function usePrefetchTreatments(): void {
  const queryClient = useQueryClient();
  queryClient.prefetchQuery(queryKeys.treatments, getTreatments);
}
```
