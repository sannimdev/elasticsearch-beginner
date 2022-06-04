# Pagination

-   이전에 다루지 않았던 내용은 아니며 페이징을 구현하기 위해 다시 강의

## 페이징 원리

```js
total_pages = Math.ceil(total_hits / page_size);
14 = Math.ceil(137 / 10);
from = (page_size * (page_number-1));
50 = (10*(6-1))
```
