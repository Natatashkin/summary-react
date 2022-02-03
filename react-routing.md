- [2:00](https://youtu.be/IY_btZ2pYpw?t=119) Концепция SPA (Single Page Application) и CSR (Client Side Rendering).
- HTML5 History API
- [12:34](https://youtu.be/IY_btZ2pYpw?t=754) Компонент `BrowserRouter`
- Меню навигации
  - [15:22](https://youtu.be/IY_btZ2pYpw?t=922) Компонент `Link`
  - [18:27](https://youtu.be/IY_btZ2pYpw?t=1107) Компонент `NavLink`
  - [20:03](https://youtu.be/IY_btZ2pYpw?t=1203) Оформление и проп `exact`
- [24:48](https://youtu.be/IY_btZ2pYpw?t=1488) Компонент `Route`
- [30:51](https://youtu.be/IY_btZ2pYpw?t=1851)Обработка 404
- [31:51](https://youtu.be/IY_btZ2pYpw?t=1911)Компонент `Switch`
- [39:58](https://youtu.be/IY_btZ2pYpw?t=2398)Динамические URL-параметры
  - [42:41](https://youtu.be/IY_btZ2pYpw?t=2559)Делаем вложенную навигацию c `useRouteMatch`
    Для вложеной навигации всегда используем свойство url объекта, который возвращает `useRouteMatch`
  - Свойство `match.url`
  - [46:05](https://youtu.be/IY_btZ2pYpw?t=2765)Маршрут одной книги с `useParams`
- [1:06:12](https://youtu.be/IY_btZ2pYpw?t=3972)Вложенные маршруты
  - Вложенная навигация по авторам
  - Вложенный маршрут автора с `match.path`

## Маршруты

- `/` - приветственная страница
- `/authors` - все авторы
- `/authors/:authorId` - автор и его книги
- `/books` - все книги
- `/books/:bookId` - одна книга
