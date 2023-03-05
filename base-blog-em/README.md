# Blog-em Ipsum

### useQuery

특정 쿼리의 상태를 사용자에게 알려줄 때 사용합니다.

### isFetching , isLoading

isFetching은 isLoading을 포함하는 개념입니다.  
isFetching에서는 캐시된 데이터의 쿼리가 호출 중일 수 있습니다.
isLoading은 쿼리가 만들어지지도 않았을 수 있어서 캐시된 데이터도 없습니다.  
react query에서 error로 명시하기까지 기본 세번은 시도를 한 후 error를 정의합니다.

- isFetching : aynce쿼리 함수가 해결되지 않았을 때, 참에 해당합니다. 아직 데이터를 가져오는 중임을 나타냅니다.
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
- isLoading이지만, isFetching은 아닙니다.
- 기본적으로 재시도는 없습니다.(useQuery는 3번재시도하는 것에 비해)
