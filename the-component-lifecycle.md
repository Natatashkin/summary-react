[00:25](https://youtu.be/w6MW1szKuT4?t=25) - МЕТОДЫ ЖИЗНЕНОГО ЦИКЛА
<br/>
[Жизненный цикл компонент-классов](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)
<br/>
<strong>У каждого компонента класса есть 3 фазы жизненного цикла:</strong>

1. <strong>[Монтирование](https://ru.reactjs.org/docs/react-component.html#componentdidmount)</strong> - когда компонент создаётся в первый раз и добавляется в DOM-дерево (от создания до монтирования). Выполняется 1 раз за весь жизненный цикл.
   Вызываемые методы:

`conctructor() -> render() -> ­React обновляет ­D­O­M и рефы -> componentDidMount()`

`componentDidMount()` - вызывается сразу после монтирования (то есть, вставки компонента в DOM). Этот метод гарантирует вызов методов класса в указанном порядке. В этом методе должны происходить действия, которые требуют наличия DOM-узлов. Это хорошее место для создания сетевых запросов. Не нужно делать публичным свойством класса. Не передаётся как колбек.

2. <strong>[Обновление](https://ru.reactjs.org/docs/react-component.html#componentdidupdate)</strong> - класс обновляется, когда приходят новые пропсы или меняется state.

`render() -> ­React обновляет ­D­O­M и рефы -> componentDidMount()`

`componentDidUpdate(prevProps, prevState, snapshot)` - вызывается сразу после каждого обновления. Не вызывается при первом рендере.

Метод позволяет работать с DOM при обновлении компонента. Также он подходит для выполнения таких сетевых запросов, которые выполняются на основании результата сравнения текущих пропсов с предыдущими. Если пропсы не изменились, новый запрос может и не требоваться.

3. <strong>[Размонтирование](https://ru.reactjs.org/docs/react-component.html#componentwillunmount)</strong> вызывается 1 раз перед тем, как элемент удаляется из DOM-дерева.

`componentWillUnmount()` вызывается непосредственно перед размонтированием и удалением компонента. В этом методе выполняется необходимый сброс: отмена таймеров, сетевых запросов и подписок, созданных в componentDidMount().

Не используйте setState() в componentWillUnmount(), так как компонент никогда не рендерится повторно. После того, как экземпляр компонента будет размонтирован, он никогда не будет примонтирован снова.

[06:37](https://youtu.be/w6MW1szKuT4?t=397) - Редкие методы

- [shouldComponentUpdate()](https://ru.reactjs.org/docs/react-component.html#shouldcomponentupdate)
  Этот метод нужен только для повышения производительности.
- [getSnapshotBeforeUpdate()](https://ru.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
  Позволяет вашему компоненту брать некоторую информацию из DOM (например, положение прокрутки) перед её возможным изменением.

  - [static getDerivedStateFromProps()](https://ru.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
    Саша рекомендует не трогать.

[09:30](https://youtu.be/w6MW1szKuT4?t=570) - Сохранение коллекции заметок в localStorage (componentDidMount и componentDidUpdate)

В componentDidUpdate можно пролучить доступ к прошлому (prevState) и текущему(this.state) значению state. Можно сравнить:
<br/>
<code>

if(this.state.todos !== prevState.todos) {
localStorage.setItem('todos', JSON.stringify(this.state.todos));
};

</code>
В этом методе или в render() нельзя делать setState() не внутри какого-то условия, во избежание бесконечного цикла: меняется state-> выполняется render()-> вызывается componentDidUpdate и по кругу

[16:37](https://youtu.be/w6MW1szKuT4?t=997) получаем данные из localStorage при загрузке страницы

Получаем из componentDidMount()
<br/>
<code>
componentDidMount() {
const todos = localStorage.getItem('todos');
const parsedTodos = JSON.parse(todos);

        // если в localStorage ничего нет, тогда parsedTodos вернёт null и сломается вся логика, потому нужно условие

    if (parsedTodos) {
        this.setState({ todos: parsedTodos }); // записываем поверх, так как в начальном стойте пустые значения
    }

}
</code>

- Модальное окно (componentDidMount и componentWillUnmount)
  - Проблема z-index, как решать без костылей (порталы)
  - Слушатель на keydown для Escape
  - Слушатель на клик по Backdrop
- Таймер и утечка памяти с setState() без componentWillUnmount
- Табы (shouldComponentUpdate)
  - аналогия с колорпикером = главное знать технику
  - убираем лишние рендеры после setState
- Рефакторим заметки
  - Выносим туду в отдельный компонент
  - Кнопка-иконка и импорт SVG как ReactComponent
  - Убираем редактор в модальное окно
