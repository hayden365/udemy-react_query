# Infinite SWAPI

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
