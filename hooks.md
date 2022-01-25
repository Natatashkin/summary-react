# Хуки, часть 1

- [Документация](https://ru.reactjs.org/docs/hooks-intro.html)
- useState
  - SignupForm
    - Одно значение = один useState
  - ColorPicker
- useEffect
  - Counter
    - Каждый рендер
    - Каждый рендер при изменении какого-то стейта или пропса
  - SignUpForm
    - localStorage
    - lazy state initialization
    - Кастомный хук useLocalStorage
- useRef
  - Clock
    - Первый рендер
    - Последний рендер
- useContext
  - UserMenu
- Покемоны

## Библиотеки

- [React Hook Form](https://react-hook-form.com/)
- [Хуки для HTTP-запросов](https://github.com/tannerlinsley/react-query)
- [Кастомные хуки](https://github.com/streamich/react-use)

[3:59](https://youtu.be/S1NrJZscSrc?t=234) - <strong id='1'> useState </strong>

- SignupForm
  Импортируем {useState} из реакта.
  В функции-компоненте объявляем состояние: `const inputState = useState();`
  В ней передаётся начальное состояние нашего компонента: `useState('');`, например, пустая строка. Если вывести console.log(inputState), то мы увидим в консоли массив из двух значений:` ['', f]`, где `''` - текущее значение состояния, а `f` - функция апдэйтер этого значения.
  Можно деструктуризировать: `const [email, setEmail] = useState();` Контекст не нужен, для обработчиков пишется просто функция и переменные, как и функции передаються в разметку как обычно, без `this`

<pre><code>
export default function SignupForm() {
  const [email, setEmail] = useLocalStorage('email', '');
  const [password, setPassword] = useLocalStorage('password', '');

  const handleChange = event => {
      setEmail(event.target.value)
  };

  return (
    &lt;form className={styles.form} autoComplete="off"&gt;
      &lt;label className={styles.label}&gt;
        &lt;span&gt;Почта&lt;/span&gt;
        &lt;input
          type="email"
          name="email"
          onChange={handleChange}
          value={email}
        /&gt;
      &lt;/label&gt;

      &lt;button type="submit"&gt;Зарегистрироваться&lt;/button&gt;
    &lt;/form&gt;

);
}
</code></pre>

- [10:50](https://youtu.be/S1NrJZscSrc?t=650) - Одно значение = один useState
  В хуках не хранят объекты состояний. На каждое состояние отдельный `useState()`
- [20:04](https://youtu.be/S1NrJZscSrc?t=1204) - ColorPicker

[30:27](https://youtu.be/S1NrJZscSrc?t=1827) - useEffect

- Counter
  - Каждый рендер
  - Каждый рендер при изменении какого-то стейта или пропса

prevState и state - просто параметры(в примере). обновление от предыдущего.
useEffect ни во что нельзя вложить. Первым аргументом передаётся ТОЛЬКО АНОНИМНАЯ ФУНКЦИЯ!!!! никаких колбэков.

Вторым аргументом, в него можно передать массив зависимостей.
<br/>

- Если массив пустой, то useEffect не зависит ни от чего, запуститься 1 раз при первом рендере
  <br/>
- Если массива нет, то это значит что useEffect выполняеться каждый раз при каждом чихе
  <br/>
- Если передать параметры, то будет изменяться при изменении переданных параметров, например `useEffect(() => { const totalClicks = counterA + counterB; document.title = 'Всего кликнули ${totalClicks} раз'; }, [counterA, counterB]);`. Это значит: если что-то изменилось, запусти эту функцию.

Пример

<pre><code>
export default function Counter() {
const [counterA, setCounterA] = useState(0);
const [counterB, setCounterB] = useState(0);

// const handleCounterAIncrement = () => {
// setCounterA(prevState => prevState + 1);
// };

const handleCounterAIncrement = () => {
setCounterA(state => state + 1);
};

// const handleCounterBIncrement = () => {
// setCounterB(prevState => prevState + 1);
// }; 

const handleCounterBIncrement = () => {
setCounterB(state => state + 1);
};

useEffect(() => {
const totalClicks = counterA + counterB;
document.title = `Всего кликнули ${totalClicks} раз`;
}, [counterA, counterB]);

return (
&lt;&gt;
    &lt;button
    className={styles.btn}
    type="button"
    onClick={handleCounterAIncrement}
    &gt;
    Кликнули counterA {counterA} раз
    &lt;/button&gt;

      &lt;button
        className={styles.btn}
        type="button"
        onClick={handleCounterBIncrement}
      &gt;
        Кликнули counterB {counterB} раз
      &lt;/button&gt;
    &lt;/&gt;

);
}
</code></pre>

- [52:07](https://youtu.be/S1NrJZscSrc?t=3116) - SignUpForm

  - [53:27](https://youtu.be/S1NrJZscSrc?t=3221) - localStorage

  - [1:01:16](https://youtu.be/S1NrJZscSrc?t=3676)- lazy state initialization
    <pre><cod>
    const [email, setEnail] = useState(()=> JSON.parse(window.localStorage.getItem('email)) ?? '')
    </code></pre>

  - [1:05:36](https://youtu.be/S1NrJZscSrc?t=3936) - Кастомный хук useLocalStorage
    Кастомный хук - это обычная функция. Кастомные хуки можно выносить в отдельную папочку и импортировать.

  `hook`

 <pre><code> import { useState, useEffect } from 'react';
export default function useLocalStorage(key, defaultValue) {
const [state, setState] = useState(() => {
return JSON.parse(window.localStorage.getItem(key)) ?? defaultValue;
});
useEffect(() => {
window.localStorage.setItem(key, JSON.stringify(state));
}, [key, state]);
return [state, setState];
}</code></pre>

`App`

<pre><code>
export default function SignupForm() {
  const [email, setEmail] = useLocalStorage('email', '');
  const [password, setPassword] = useLocalStorage('password', '');
  и т.д.
  </code></pre>

[1:20:58](https://youtu.be/S1NrJZscSrc?t=4858) - useRef
Запомнить! Если начальное значение стейта - вычисляемое выражение, делаем его ленивым `useState(()=> new Date())`, тогда оно сработает один рах при инициализации проекта

- [1:21:41](https://youtu.be/S1NrJZscSrc?t=4901) - Clock
  [1:23:54](https://youtu.be/S1NrJZscSrc?t=5034) - проблема intervalId
  [1:28:01](https://youtu.be/S1NrJZscSrc?t=5281) - решение: для того чтобы сохранить стабильное значение(персистное значение) между рендерами используются рефы. Они рендеряться один раз и не обновляются. Исполбзуется {useRef}

  `const intervalId = useRef(null)` - вернёт свойство {current: null} или что мы туда впишем.

      <pre><code>
      export default function Clock() {
      const [time, setTime] = useState(() => new Date());
      const intervalId = useRef(null); - Первый рендер

      useEffect(() => {
          // intervalId.current - записываем id интервала
        intervalId.current = setInterval(() => {
          console.log('Это интервал каждые 2000ms ' + Date.now());
          setTime(new Date());
        }, 2000);

  // Последний рендер
  return () => {
  console.log('Это функция очистки перед следующим вызовом useEffect');
  stop();
  };
  }, []);

      const stop = () => {
        clearInterval(intervalId.current);
      };

      return (
        &lt;div className={styles.container}&gt;
          &lt;p className={styles.clockface}&gt;
            Текущее время: {time.toLocaleTimeString()}
          &lt;/p&gt;
          &lt;button type="button" onClick={stop}&gt;
            Остановить
          &lt;/button&gt;
        &lt;/div&gt;
      );

  }
  </code></pre>

  <br/> - [1:34:25](https://youtu.be/S1NrJZscSrc?t=5665) - Последний рендер
  Пример выше. То, что возвращается из useEffect віполняется перед размонтированием компонента.

[](https://youtu.be/S1NrJZscSrc?t=6521) - Вопросы

- [1:48:36](https://youtu.be/S1NrJZscSrc?t=6521) - когда в пропс передаётся вызов функции, а когда колбек?<br/>
  1. когда нам нужно вызвать функцию прям здесь и сейчас - вызов функции: {function()}<br/>
  2. когда нам нужно, чтобы функция вызвалась по какому-то событию, мы передаём вызов по ссылке `{()=> function()}` <br/>
- [1:52:53](https://youtu.be/S1NrJZscSrc?t=6773) - как передавать один и тот же фетч в хуках в разных участках кода?
  <pre>useEffect(() => {
  // тут всё, что касается http
  }, [key, state]); // когда это вызвать</pre>

  например есть кнопка:
  <pre>&lt;button onClick={()=> setPage(page=> page+1)}&gt;Load more&lt;/button&gt;</pre>

  Мы просто меняем значение страницы по клику, а состояние контролируется `useEffect`.

[1:58:12](https://youtu.be/S1NrJZscSrc?t=7092) - useContext

- UserMenu, с хуками работают react-hook-form
  [2:13:35](https://youtu.be/S1NrJZscSrc?t=8015) - сделать модалку на хуках

  []() - Покемоны

## Доп вебинар, Виджет новостей

[1:11:06](https://youtu.be/GQoPaL1BgaM?t=4266) - форма.
[1:14:26](https://youtu.be/GQoPaL1BgaM?t=4458) - fetch
[1:28:40](https://youtu.be/GQoPaL1BgaM?t=5320) - кнопка Load More
[1:31:12](https://youtu.be/GQoPaL1BgaM?t=5473) - Ловим ошибку

- [1:32:48](https://youtu.be/GQoPaL1BgaM?t=5568) - Решаем проблему постоянной магической отправки фетчей - решайте сами короче, читайте доки.

[1:40:33](https://youtu.be/GQoPaL1BgaM?t=5973) - Контекст
