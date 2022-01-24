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
    <form className={styles.form} autoComplete="off">
      <label className={styles.label}>
        <span>Почта</span>
        <input
          type="email"
          name="email"
          onChange={handleChange}
          value={email}
        />
      </label>

      <button type="submit">Зарегистрироваться</button>
    </form>
  );
}
</code></pre>

    - SignupForm
        - Одно значение = один useState
    - ColorPicker
