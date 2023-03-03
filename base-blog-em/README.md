# Blog-em Ipsum

### isFetching , isLoading

isFetching은 isLoading을 포함하는 개념입니다.  
isFetching에서는 캐시된 데이터의 쿼리가 호출 중일 수 있습니다.
isLoading은 쿼리가 만들어지지도 않았을 수 있어서 캐시된 데이터도 없습니다.  
react query에서 error로 명시하기까지 기본 세번은 시도를 한 후 error를 정의합니다.

### staleTime, cacheTime

staleTime은 만료시간을 정의하고 만료되었다면 데이터를 리페칭하게 됩니다. 기본값을 0입니다. 만료된 데이터 외에도 항상 서버에서 가져오게 하도록 하는 설정이 가능합니다.  
cacheTime은 데이터의 유효시간이며, 기본값은 5분입니다. useQuery가 활성화된 후 부터 경과한 시간입니다. 서비스특성상 만료된 데이터가 위험한 경우 cacheTime을 0으로 설정하면 데이터가 만료된 경우를 방지할 수 있습니다.
