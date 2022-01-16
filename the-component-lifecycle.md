<ol>
  <li><a href='#1'>МЕТОДЫ ЖИЗНЕНОГО ЦИКЛА</a></li>
  <li><a href='#2'>Редкие методы</a></li>
  <li><a href='#3'>Сохранение коллекции заметок в localStorage (componentDidMount и componentDidUpdate)</a></li>
  <li><a href='#4'>Получаем данные из localStorage при загрузке страницы</a></li>
  <li><a href='#5'>Модальное окно (componentDidMount и componentWillUnmount)</a>
      <ul>
        <li><a href='#6'>Проблема z-index, как решать без костылей (порталы)</a></li>
        <li><a href='#7'>Слушатель на keydown для Escape</a></li>
        <li><a href='#8'>Слушатель на клик по Backdrop</a></li>
      </ul>
  </li>
  <li><a href='#9'>Таймер и утечка памяти с setState() без componentWillUnmount</a></li>
  <li><a href='#10'>Табы (shouldComponentUpdate)</a>
      <ul>
        <li><a href='#11'>аналогия с колорпикером = главное знать технику</a></li>
        <li><a href='#12'>убираем лишние рендеры после setState</a></li>
        <li><a href='#13'>PureComponent</a></li>
      </ul>
  </li>
  <li><a href='#14'>Рефакторим заметки</a>
      <ul>
        <li><a href='#15'>Выносим туду в отдельный компонент</a></li>
        <li><a href='#16'>Кнопка-иконка и импорт SVG как ReactComponent</a></li>
        <li><a href='#16'>Убираем редактор в модальное окн</a></li>
      </ul>
  </li>
</ol>

[00:25](https://youtu.be/w6MW1szKuT4?t=25) - <strong id="1">МЕТОДЫ ЖИЗНЕНОГО ЦИКЛА</strong>
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

[06:37](https://youtu.be/w6MW1szKuT4?t=397) - <strong id='2'>Редкие методы</strong>

- [shouldComponentUpdate(nextProps, nextState)](https://ru.reactjs.org/docs/react-component.html#shouldcomponentupdate)
  Этот метод нужен только для повышения производительности.
- [getSnapshotBeforeUpdate(prevProps, prevState)](https://ru.reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
  Позволяет вашему компоненту брать некоторую информацию из DOM (например, положение прокрутки) перед её возможным изменением.
- [static getDerivedStateFromProps()](https://ru.reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
  Саша рекомендует не трогать.

[09:30](https://youtu.be/w6MW1szKuT4?t=570) - <strong id='3'>Сохранение коллекции заметок в localStorage (componentDidMount и componentDidUpdate)</strong>

В `componentDidUpdate(prevProps, prevState, snapshot)` можно пролучить доступ к прошлому (prevState) и текущему(this.state) значению state. Можно сравнить:
<br/>

<pre><code>
if(this.state.todos !== prevState.todos) {
localStorage.setItem('todos', JSON.stringify(this.state.todos));
};
</code></pre>

В этом методе или в render() нельзя делать setState() не внутри какого-то условия, во избежание бесконечного цикла: меняется state-> выполняется render()-> вызывается componentDidUpdate и по кругу

[16:37](https://youtu.be/w6MW1szKuT4?t=997) - <strong id='4'>Получаем данные из localStorage при загрузке страницы</strong>

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

[23:53](https://youtu.be/w6MW1szKuT4?t=1373) - <strong id='5'>Модальное окно (componentDidMount и componentWillUnmount)</strong>
Стандартная разметка с бэкдопом и лайтбоксом внутри.
В стейт добавляем свойство showModal: false и пишем обработчик, который перезаписывает состояние showModal с false на true и наоборот. Модалку рендерим по условиюЖ нажато что-то - показываем модалку.
Компоненты модалки передаются как чилдрены через пропсы.

[30:27](https://youtu.be/w6MW1szKuT4?t=1827)- <strong id='6'>Проблема z-index, как решать без костылей (порталы)</strong>

1.Нужно в index.html создать портал под дивом root
`<div id="modal-root"></div>`

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
  &lt;div className="Modal__backdrop" onClick={this.handleBackdropClick}&gt;
  &lt;div className="Modal__content"&gt;{this.props.children}&lt;/div&gt;
  &lt;/div&gt;
  ,
  modalRoot,
  );
  }
  </code></pre>

В результате модалка рендериться в другом руте и решается проблема с z-index.

[30:15](https://youtu.be/w6MW1szKuT4?t=2115)- <strong id='7'>Слушатель на keydown для Escape</strong><br/>
1.В реакте нельзя повесить слушатель на window. Для этого используем componentDidMount(). Это один из немногих случаев, когда мы используем `addEventListener`

  <pre>componentDidMount() {
      console.log('Modal componentDidMount');
      window.addEventListener('keydown', this.handleKeyDown);
    }</pre>

2.Пишем обрабработчик, как метод класса Modal this.handleKeyDown на нажатие ESС. В него вставляем переключалку модалки написаную ранее как пропс. Пропс передаём в компонент модалки

  <pre>this.props.onClose();</pre>

3.После этого модалка будет реагировать на escape даже при простом нажатии esc. Нужно снять обработчик window.addEventListener('keydown', this.handleKeyDown). Снимаем слушатель в `componentWillUnmount()`. Непочищенные слушатели валят производительность.

[43:50](https://youtu.be/w6MW1szKuT4?t=2630)- <strong id='8'>Слушатель на клик по Backdrop</strong>

1.Пишем обработчик и передам пропсом в бэкдроп. <br/>
2.Исключить клики по лайтбоксу(e.currentTarget, e.target сравнить).

[48:10](https://youtu.be/w6MW1szKuT4?t=2890)- <strong id='9'>Таймер и утечка памяти с setState() без componentWillUnmount</strong>

<pre><code>
export default class Clock extends Component {
  state = {
    time: new Date().toLocaleTimeString(),
  };

  intervalId = null;

  componentDidMount() {
    console.log('setInterval');

    this.intervalId = setInterval(
      () => this.setState({ time: new Date().toLocaleTimeString() }),
      1000,
    );
  }

// при скрытии таймера нбх убрать intervalId, т.к. он продолжает работать даже на размонтированном компоненте, что приводит к утечке памяти
  componentWillUnmount() {
    clearInterval(this.intervalId);
  }

  render() {
    return <div className="Clock__face">{this.state.time}</div>;
  }
}
</code></pre>

[54:44](https://youtu.be/w6MW1szKuT4?t=3284)- <strong id='10'>Табы (shouldComponentUpdate)</strong>

[57:00](https://youtu.be/w6MW1szKuT4?t=3420)- <em id='11'>аналогия с колорпикером = главное знать технику</em>

[59:03](https://youtu.be/w6MW1szKuT4?t=3546)- <em id='12'>убираем лишние рендеры после setState</em>

При клике на одну и ту же кнопку неск раз подряд, перерендеривается разметка. Мы можем остановить ре-рендер при одинаковом стайте. Используем метод `shouldComponentUpdate()`, но лучше не использовать.
Этот метод вызывается перед методом render() после обновления стейта. Если он вернёт false, то ре-рендер не произойдёт.

<pre><code>
shouldComponentUpdate(nextProps, nextState) {
  return nextState.activeTabIdx !== this.state.activeTabIdx;
}
</code></pre>

[1:04:16](https://youtu.be/w6MW1szKuT4?t=3856) - <em id='13'>PureComponent</em>

PureComponent - чистый компонент. У него под капотом реализовано `shouldComponentUpdate()` и идёт поверхнорстное сравнение всех пропсов. Импортировать в файл вместо Component. Лучше использовать его, если нужно контролировать ре-рендер.

<pre><code>
import React, { PureComponent } from 'react';

export default class Tabs extends PureComponent {}
</code></pre>

[1:08:44](https://youtu.be/w6MW1szKuT4?t=4122)- <strong id='14'>Рефакторим заметки</strong>

[01:09:13](https://youtu.be/w6MW1szKuT4?t=4153) - <em id='15'>Выносим туду в отдельный компонент</em>

<pre><code>
<Todo
  text={text}
  completed={completed}
  onToggleCompleted={() => onToggleCompleted(id)} //Происходит привязка функции к id, а в компонент уже прийдёт ссылка на вызов этой функции.
  onDelete={() => onDeleteTodo(id)}
/>
</code></pre>

[1:14:07](https://youtu.be/w6MW1szKuT4?t=4448) - <em id='16'>Кнопка-иконка и импорт SVG как ReactComponent</em>
<br/>
1.Создаём компонент кнопку IconButton:

<pre><code>
const IconButton = ({ children, onClick, ...allyProps }) => (
  <button type="button" className="IconButton" onClick={onClick} {...allyProps}>
    {children}
  </button>
);

IconButton.defaultProps = {
  onClick: () => null,
  children: null,
};

IconButton.propTypes = {
  onClick: PropTypes.func,
  children: PropTypes.node,
  'aria-label': PropTypes.string.isRequired,
};

export default IconButton;
</code></pre>

2.передаём пропсы в компонент
`<IconButton onClick={this.toggleModal} aria-label="Добавить todo"></IconButton>`
3.В папке `components` делаем папку icons в которой лежат все svg. Спрайт леоать не обязательно.
Импортируем svg как реакт компонент:
`import {ReactComponent as AddIcon} from ".. путь к иконке"`, при этом `AddIcon` - произвольное название компонента
Импортируем в файл где вставляем в компонет кнопку.
4.Вставляем в кнопку как компонент то, чот мы импортировали

<pre><code>
<IconButton onClick={this.toggleModal} aria-label="Добавить todo">
          <AddIcon width="40" height="40" fill="#fff" />
        </IconButton>
</code></pre>

[1:21:44](https://youtu.be/w6MW1szKuT4?t=4904) - <em id='17'>Убираем редактор в модальное окно</em>

Если вызывать функцмю закрытия модалки в функции добавления тудушек, получается более связанный код. Это можно сделать в методе componentDidUpdate, добавив проверки.

<pre><code>
 if (nextTodos.length > prevTodos.length && prevTodos.length !== 0) {
      this.toggleModal();
    }
    </code></pre>

Что грузит апп? Проверка одного объекта с другим по всем свойствам.

[1:31:30](https://youtu.be/w6MW1szKuT4?t=5490)- aria-label

aria-label-атрибут доступности. Реакт пропускаеи с тире в названии. Мы его передаём как пропс, в компоненте собираем как ...allyProps и потом в компонент распыляем. Это обязательно для кнопок-иконок.

<pre><code>
const IconButton = ({ children, onClick, ...allyProps }) => (
  <button type="button" className="IconButton" onClick={onClick} {...allyProps}>
  </code></pre>
