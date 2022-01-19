- <a href="#1">Основы HTTP-запросов в React</a>
- <a href="#2">Loading...</a>
- <a href="#3">«Архитектура» компонентов и состояний</a>
- <a href="#4">Форма для имени покемона PokemonForm</a>
- <a href="#5">Импорт собственных иконок svg в React</a>
  - <a href="#6">Форму нельзя отправить если имя покемона пустое</a>
  - <a href="#7">`react-toastify` для алертов</a>
- <a href="#8">Компонент `App`</a>
  - <a href="#9">Нужен стейт для хранения имени покемона</a>
- <a href="#10">Информация о покемоне `PokemonInfo`</a>
  - <a href="#11">Нужей стейт для хранения ответа от бекенда</a>
  - <a href="#12">Ендпоинт покемонов `https://pokeapi.co/api/v2/pokemon/имя_покемона`</a>
  - <a href="#13">Рендер разметки</a>
    - <a href="#14">Если нет имени покемона показывать текст «Введите имя покемона.»</a>
    - <a href="#15">Если загружаем то показываем текст «Загружаем...»</a>
    - <a href="#16">Если есть покемон, показываем карточку покемона</a>
    - <a href="#17">Если загружаем то показываем фолбек компонент</a>
  - <a href="#18">Обработка ошибки HTTP-запроса</a>
    - <a href="#19">В `fetch` нужно обработать 404 в ответе</a>
    - <a href="#20">Возвращаем `Promise.reject(new Erorr(Нет покемона с именем имя_покемона))`</a>
    - <a href="#21">Нужен стейт для хранения ошибки</a>
    - <a href="#22">Если ошибка рендерим разметку ошибки</a>
- <a href="#23">Паттерн state machine</a>
- <a href="#24">Структурируем и оптимизируем код</a>
- <a href="#25">Скелетон, спинер</a>
  <hr/>

[4:11](https://youtu.be/xoG3l2PgiYY?t=251) - <strong id="1">Основы HTTP-запросов в React</strong>
componentDidMount() -> fetch(url).then(res=>res.json).then(pokemon=> this.setState({pokemon})). Установить начальное состояние покемона в state null.

[9:11](https://youtu.be/xoG3l2PgiYY?t=575) - <strong id="2">Loading...</strong>

Добавить в стейт состояние loading: false и манипулировать в componentDidMount()

[12:33](https://youtu.be/xoG3l2PgiYY?t=753) - <strong id="3">«Архитектура» компонентов и состояний</strong>

Всё стёрли, теперь всё по-другому ))))
Для формы поиска нужен локальный стейт.

[15:46](https://youtu.be/xoG3l2PgiYY?t=946) - <strong id="4">Форма для имени покемона PokemonForm</strong>

- [16:12](https://youtu.be/xoG3l2PgiYY?t=985) - <span id="5">Импорт собственных иконок svg в React</span>

- [23:33](https://youtu.be/xoG3l2PgiYY?t=1413) - <span id="6">Форму нельзя отправить, если имя покемона пустое</span>
  условие `this.state.pokemonName.trim() === '';` с выходом

- [26:30](https://youtu.be/xoG3l2PgiYY?t=1590) - <span id="7">`react-toastify` для алертов</span>

[30:51](https://youtu.be/xoG3l2PgiYY?t=1851) - <strong id="8">Компонент `App`</strong>

- [20:14](https://youtu.be/xoG3l2PgiYY?t=1334) - <span id="9">Нужен стейт для хранения имени покемона</span>

[30:57](https://youtu.be/xoG3l2PgiYY?t=1857) - <strong id="10">Информация о покемоне `PokemonInfo`</strong>
Тут делаем fetch, т.к. больше нигде в компонентах данные фетча не нужны.
Этот компонент получает 1 прод pokemonName из App

- [40:02](https://youtu.be/xoG3l2PgiYY?t=2402) - <span id="11">Нужей стейт для хранения ответа от бекенда</span>

- [33:41](https://youtu.be/xoG3l2PgiYY?t=2018) - <span id="12">Ендпоинт покемонов `https://pokeapi.co/api/v2/pokemon/имя_покемона`</span>

componentDidUpdate() - сравниваем предыдущее значение имени покемона с текущим и по условию делаем фетч за новым покемоном(тот что при сравнении текущий)

- [40:58](https://youtu.be/xoG3l2PgiYY?t=2458) - <span id="13">Рендер разметки</span>
  - [41:54](https://youtu.be/xoG3l2PgiYY?t=2514) - <span id="14">Если нет имени покемона показывать текст «Введите имя покемона.»</span>
  - [42:54](https://youtu.be/xoG3l2PgiYY?t=2574) - <span id="15">Если загружаем то показываем текст «Загружаем...»</span>
    Добавляем стейт для загрузки, false. В методе componentDidUpdate перед запросом меняем на true, цепляем finally и в неём возвращаем false
  - [46:31](https://youtu.be/xoG3l2PgiYY?t=2791) - <span id="16">Если есть покемон, показываем карточку покемона</span>
  - []() - <span id="17">Если загружаем то показываем фолбек компонент</span>
- [46:54](https://youtu.be/xoG3l2PgiYY?t=2814) - <span id="18">Обработка ошибки HTTP-запроса</span>

  - [49:19](https://youtu.be/xoG3l2PgiYY?t=2957) - <span id="19">В `fetch` нужно обработать 404 в ответе</span>
    Чтобы отловить 404 в state нужно хранить ответ ошибки: `error: null`. В fetch добавляем catch(error=> this.setState({error})), связываем состояние.Добавляем рендер по условию. В данном примере сработал отлов ошибки, т.к. бекэнд ручками предусмотрел этот статус. В том случае, если это не предусмотрено бэкендом прокидываем ошибку вручную
  - [51:57](https://youtu.be/xoG3l2PgiYY?t=3118) - <span id="20">Возвращаем `Promise.reject(new Erorr(Нет покемона с именем имя_покемона))`</span>
    <pre><code>
      .then(resolve => {
    if(response.ok) {
      return response.json;
    }
    Promise.reject(new Erorr(Нет покемона с именем имя_покемона))
    })
    </code></pre>
  - [49:19](https://youtu.be/xoG3l2PgiYY?t=2957) - <span id="21">Нужен стейт для хранения ошибки</span>
    Чтобы пока происходит новый поиск покемена, старый покемон не мозолил глаза, беред каждым фетчем в стайте нужно сбросить значение покемона в null
  - [56:23](https://youtu.be/xoG3l2PgiYY?t=3383) - <span id="22">Если ошибка рендерим разметку ошибки</span>

[59:12](https://youtu.be/xoG3l2PgiYY?t=3552) - <strong id="23">Паттерн state machine</strong>

- Плюсы
  - Уходят проблемы сброса полей «чтобы работало».
  - Не нужно следить на значениями N полей.
  - Более понятные условия рендера разметки.
- Используем статус
  - idle - запроса еще нет
  - pending - пошел запрос
  - resolved - успешный запрос
  - rejected - запрос с ошибкой

[1:08:10](https://youtu.be/xoG3l2PgiYY?t=4090) - <strong id="#24">Структурируем и оптимизируем код</strong>

[1:13:32](https://youtu.be/xoG3l2PgiYY?t=4412) - <strong id="#25">Скелетон</strong>
