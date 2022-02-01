# Хуки, часть 1

- [Документация](https://ru.reactjs.org/docs/hooks-intro.html)
- <a href="#1">useState</a>
  - <a href="#1">SignupForm</a>
    - <a href="#2">Одно значение = один useState</a>
  - <a href="#3">ColorPicker</a>
- <a href="#4">useEffect</a>
  - <a href="#4">Counter</a>
    - <a href="#4">Каждый рендер</a>
    - <a href="#4">Каждый рендер при изменении какого-то стейта или пропса</a>
  - <a href="#5">SignUpForm</a>
    - <a href="#6">localStorage</a>
    - <a href="#7">lazy state initialization</a>
    - <a href="#8">Кастомный хук useLocalStorage</a>
- <a href="#9">useRef</a>
  - <a href="#10">Clock</a>
    - <a href="#11">проблема intervalId</a>
    - <a href="#12">решение:</a>
    - <a href="#13">Первый рендер</a>
    - <a href="#14">Последний рендер</a>
- <a href="#15">Вопросы</a>
  - <a href="#16">когда в пропс передаётся вызов функции, а когда колбек?</a>
  - <a href="#17">как передавать один и тот же фетч в хуках в разных участках кода?</a>
- <a href="#18">useContext</a>
  - <a href="#19">UserMenu</a>
  - <a href="#20">Cделать модалку на хуках</a>
- <a href="#21">Доп вебинар, Виджет новостей</a>
  - <a href="#22">форма</a>
  - <a href="#23">fetch</a>
  - <a href="#24">кнопка Load More</a>
  - <a href="#25">Ловим ошибку</a>
  - <a href="#26">Решаем проблему постоянной магической отправки фетчей</a>
  - <a href="#27">Контекст</a>

## Библиотеки

- [React Hook Form](https://react-hook-form.com/)
- [Хуки для HTTP-запросов](https://github.com/tannerlinsley/react-query)
- [Кастомные хуки](https://github.com/streamich/react-use)

# Хуки, часть 2

- <a href="#28">useEffect и пропуск первого рендера</a>
- <a href="#29">Покемоны</a>
  - <a href="#30">useState</a>
  - <a href="#31">useEffect</a>
- <a href="#32">Счётчик c useReducer</a>
- <a href="#33">Мемоизация вычислений с useMemo</a>
- <a href="#34">Profiler</a>
- <a href="#35">Космпоненты, которые перерендериваются в родительской функции, но не изщменяются</a>
- <a href="#36">useLayoutEffects</a>
- [Hook flow](https://raw.githubusercontent.com/donavon/hook-flow/master/hook-flow.png)

<hr />
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

- [10:50](https://youtu.be/S1NrJZscSrc?t=650) - <span id="2">Одно значение = один useState</span>
  В хуках не хранят объекты состояний. На каждое состояние отдельный `useState()`
- [20:04](https://youtu.be/S1NrJZscSrc?t=1204) - <span id="3">ColorPicker</span>

[30:27](https://youtu.be/S1NrJZscSrc?t=1827) - <strong id="4">useEffect</strong>

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

- [52:07](https://youtu.be/S1NrJZscSrc?t=3116) - <span id="5">SignUpForm</span>

  - [53:27](https://youtu.be/S1NrJZscSrc?t=3221) - <span id="6">localStorage</span>

  - [1:01:16](https://youtu.be/S1NrJZscSrc?t=3676)- <span id="7">lazy state initialization</span>
    <pre><cod>
    const [email, setEnail] = useState(()=> JSON.parse(window.localStorage.getItem('email)) ?? '')
    </code></pre>

  - [1:05:36](https://youtu.be/S1NrJZscSrc?t=3936) - <span id="8">Кастомный хук useLocalStorage</span>
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

[1:20:58](https://youtu.be/S1NrJZscSrc?t=4858) - <strong id="9">useRef</strong>
Запомнить! Если начальное значение стейта - вычисляемое выражение, делаем его ленивым `useState(()=> new Date())`, тогда оно сработает один рах при инициализации проекта

- [1:21:41](https://youtu.be/S1NrJZscSrc?t=4901) - <span id="10">Clock</span>
  [1:23:54](https://youtu.be/S1NrJZscSrc?t=5034) - <span id="11">проблема intervalId</span>
  [1:28:01](https://youtu.be/S1NrJZscSrc?t=5281) - <span id="12">решение:</span> для того чтобы сохранить стабильное значение(персистное значение) между рендерами используются рефы. Они рендеряться один раз и не обновляются. Исполбзуется {useRef}

  `const intervalId = useRef(null)` - вернёт свойство {current: null} или что мы туда впишем.

      <pre><code>
      export default function Clock() {
      const [time, setTime] = useState(() => new Date());
      const intervalId = useRef(null); - <span id="13">Первый рендер</span>

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

  <br/> - [1:34:25](https://youtu.be/S1NrJZscSrc?t=5665) - <span id="14">Последний рендер</span>
  Пример выше. То, что возвращается из useEffect віполняется перед размонтированием компонента.

[](https://youtu.be/S1NrJZscSrc?t=6521) - <strong id="15"> Вопросы</strong>

- [1:48:36](https://youtu.be/S1NrJZscSrc?t=6521) - <span id="16">когда в пропс передаётся вызов функции, а когда колбек?</span> <br/>
  1. когда нам нужно вызвать функцию прям здесь и сейчас - вызов функции: {function()}<br/>
  2. когда нам нужно, чтобы функция вызвалась по какому-то событию, мы передаём вызов по ссылке `{()=> function()}` <br/>
- [1:52:53](https://youtu.be/S1NrJZscSrc?t=6773) - <span id="17">как передавать один и тот же фетч в хуках в разных участках кода?</span>
  <pre>useEffect(() => {
  // тут всё, что касается http
  }, [key, state]); // когда это вызвать</pre>

  например есть кнопка:
  <pre>&lt;button onClick={()=> setPage(page=> page+1)}&gt;Load more&lt;/button&gt;</pre>

  Мы просто меняем значение страницы по клику, а состояние контролируется `useEffect`.

[1:58:12](https://youtu.be/S1NrJZscSrc?t=7092) - <strong id="18">useContext</strong>

- <span id="19">UserMenu</span>, с хуками работают react-hook-form
  [2:13:35](https://youtu.be/S1NrJZscSrc?t=8015) - <span id="20">Cделать модалку на хуках</span>

## <strong id="21">Доп вебинар, Виджет новостей</strong>

[1:11:06](https://youtu.be/GQoPaL1BgaM?t=4266) - <span id="22">форма</span><br/>
[1:14:26](https://youtu.be/GQoPaL1BgaM?t=4458) - <span id="23">fetch</span><br/>
[1:28:40](https://youtu.be/GQoPaL1BgaM?t=5320) - <span id="24">кнопка Load More</span><br/>
[1:31:12](https://youtu.be/GQoPaL1BgaM?t=5473) - <span id="25">Ловим ошибку</span><br/>

- [1:32:48](https://youtu.be/GQoPaL1BgaM?t=5568) - <span id="26">Решаем проблему постоянной магической отправки фетчей</span> - решайте сами короче, читайте доки.

[1:40:33](https://youtu.be/GQoPaL1BgaM?t=5973) - <span id="27">Контекст</span><br/>

<hr/>

[0:06](https://youtu.be/KslUxJrXY3Y?t=6) - <strong id="28">useEffect и пропуск первого рендера</strong>
Нужен, например при http-запросах, если запросы происходить должны только при апдейтах.

<b>Вариант для фетчей</b>

<pre><code>
const[query, setQuery] = useState('');

useEffect(()=> {
  if(query === '') {
    return;
  }

  fetch();
}, [query])
</code></pre>

<b>В других случаях</b>

<pre><code>
const firstRender = useRef(true);

useEffect(()=> {
  if(firstRender.current) {
    firstRender.current = false;
    return;
  }
})
</code></pre>

[11:20](https://youtu.be/KslUxJrXY3Y?t=680) -<strong id="29">Покемоны</strong>
<br/>

- [11:20](https://youtu.be/KslUxJrXY3Y?t=680) - <span id="30">useState</span>
- [16:57](https://youtu.be/KslUxJrXY3Y?t=1017) - <span id="31">useEffect</span>
  В асинхронном коде порядок обновлений имеет значение [32:11](https://youtu.be/KslUxJrXY3Y?t=1930) - Коварный смех за кадром :)
  [45:26](https://youtu.be/KslUxJrXY3Y?t=2726) -<strong id="32">Счётчик c useReducer</strong>
  <br/>

    <pre><code>
    const [state, dispatch] = useReducer(reducer, initialArg, init);
    </code></pre>

  [Доки](https://ru.reactjs.org/docs/hooks-reference.html#usereducer)
  Альтернатива для useState. Принимает редюсер типа (state, action) => newState и возвращает текущее состояние в паре с методом dispatch.
  `reducer` - функция редьюса
  `initialArg` - Начальное значение
  `init` - используется, если первоначальное состояние `initialArg` нужно вычислять. чтобы это произошло 1 раз [1:00:07](https://youtu.be/KslUxJrXY3Y?t=3611)

Пример:

<pre><code>
import { useReducer } from 'react';
import styles from './Counter.module.css';

function countReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + action.payload };

    case 'decrement':
      return { ...state, count: state.count - action.payload };

    default:
      throw new Error(`Unsuported action type ${action.type}`);
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(countReducer, {
    count: 0,
  });

  return (
    &lt;div className={styles.container}&gt;
      &lt;p className={styles.value}>{state.count}&lt;/p&gt;
      &lt;button
        className={styles.btn}
        type="button"
        onClick={() => dispatch({ type: 'increment', payload: 1 })}
      &gt;
        Увеличить
      &lt;/button&gt;

      &lt;button
        className={styles.btn}
        type="button"
        onClick={() => dispatch({ type: 'decrement', payload: 1 })}
      &gt;
        Уменьшить
      &lt;/button&gt;
    &lt;/div&gt;
  );
}
</code></pre>

[1:04:22](https://youtu.be/KslUxJrXY3Y?t=3859)- <strong id="33">Мемоизация вычислений с useMemo</strong>
<br/>
`useMemo` помнит только 1 предыдущий рендер. ТОлько синхронный код.
Из `useEffect` ничего не возвращается, из `useMemo` - возвращается результат во внешний код.

[пример из лекции](https://github.com/luxplanjay/react-21-22/blob/08-%D1%85%D1%83%D0%BA%D0%B8-%D1%87%D0%B0%D1%81%D1%82%D1%8C-2/src/components/Friends.js)

[1:19:00](https://youtu.be/KslUxJrXY3Y?t=4740) - <span id="34">Profiler</span>как пользоваться и для чего Profiler в хрому в девтулзах.
Рендер должен происходить за 16 ms. 1000ms/60 кадров. Если дольше, приложение начинает тупить.
[1:27:12](https://youtu.be/KslUxJrXY3Y?t=5232) - <span id="35">Космпоненты, которые перерендериваются в родительской функции, но не изщменяются, можно обетнуть при экспорте в memo()</span>

[1:33:07](https://youtu.be/KslUxJrXY3Y?t=5587) - <span id="36">useLayoutEffects</span>
