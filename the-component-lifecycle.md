[00:25](https://youtu.be/w6MW1szKuT4?t=25) - <strong>МЕТОДЫ ЖИЗНЕНОГО ЦИКЛА</strong>
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

[06:37](https://youtu.be/w6MW1szKuT4?t=397) - <strong>Редкие методы</strong>

- [shouldComponentUpdate()](https://ru.reactjs.org/docs/react-component.html#shouldcomponentupdate)
  Этот метод нужен только для повышения производительности.
- [getSnapshotBeforeUpdate()](https://ru.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
  Позволяет вашему компоненту брать некоторую информацию из DOM (например, положение прокрутки) перед её возможным изменением.
- [static getDerivedStateFromProps()](https://ru.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
  Саша рекомендует не трогать.

[09:30](https://youtu.be/w6MW1szKuT4?t=570) - <strong>Сохранение коллекции заметок в localStorage (componentDidMount и componentDidUpdate)</strong>

В `componentDidUpdate(prevProps, prevState, snapshot)` можно пролучить доступ к прошлому (prevState) и текущему(this.state) значению state. Можно сравнить:
<br/>

<pre><code>
if(this.state.todos !== prevState.todos) {
localStorage.setItem('todos', JSON.stringify(this.state.todos));
};
</code></pre>

В этом методе или в render() нельзя делать setState() не внутри какого-то условия, во избежание бесконечного цикла: меняется state-> выполняется render()-> вызывается componentDidUpdate и по кругу

[16:37](https://youtu.be/w6MW1szKuT4?t=997) - <strong>Получаем данные из localStorage при загрузке страницы</strong>

Получаем из `componentDidMount()`
<br/>

<pre><code>
componentDidMount() {
const todos = localStorage.getItem('todos');
const parsedTodos = JSON.parse(todos);
    // если в localStorage ничего нет, тогда parsedTodos вернёт null и сломается вся логика, потому нужно условие
    if (parsedTodos) {
        this.setState({ todos: parsedTodos }); // записываем поверх, так как в начальном стойте пустые значения
    }
}
</code></pre>

[23:53](https://youtu.be/w6MW1szKuT4?t=1373) - <strong>Модальное окно (componentDidMount и componentWillUnmount)</strong>
Стандартная разметка с бэкдопом и лайтбоксом внутри.
В стейт добавляем свойство showModal: false и пишем обработчик, который перезаписывает состояние showModal с false на true и наоборот. Модалку рендерим по условиюЖ нажато что-то - показываем модалку.
Компоненты модалки передаются как чилдрены через пропсы.

- Проблема z-index, как решать без костылей (порталы)

  1.Нужно в index.html создать портал под дивом root
    <pre><code><div id="modal-root"></div></code></pre>

  2.Создаём квери селектор на это див (не в классе!)
  <pre><code>const modalRoot = document.querySelector('#modal-root');</code></pre>

  3.Импортируем
  <pre><code>import { createPortal } from 'react-dom';</code></pre>
  `ReactDOM.createPortal(child, container)`
  Создаёт портал. Порталы предоставляют способ отрендерить дочерние элементы в узле DOM, который существует вне иерархии DOM-компонента.

  4.Меняем разметку рендера
  <pre><code>
  render() {
  return createPortal(
  <div className="Modal__backdrop" onClick={this.handleBackdropClick}>
  <div className="Modal__content">{this.props.children}</div>
  </div>,
  modalRoot,
  );
  }
  </code></pre>

  В результате модалка рендериться в другом руте и решается проблема с z-index.

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
