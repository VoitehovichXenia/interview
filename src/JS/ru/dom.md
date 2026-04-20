# Типы всплывающих окон в JavaScript?

- alert(message) - выводит message на экран
- confirm(question) - запрашивает подтверждение у пользователя, возвращает true или false
- prompt(question, defaultValue) - запрашивает ввод у пользователя, возвращает введенное значение или defaultValue если ничего не было введено

# Для чего используется свойство .dataset?

.dataset используется для доступа к data аттрибутам

```js
// HTML
// <button id="custom-button" data-url="https://example.com">Button</button>
const btn = document.getElementById('custom-button')
console.log(btn.dataset.url) // https://example.com
```

# Как реализовать отложенную загрузку изображений?

1. Через HTML аттрибут loading="lazy"
```html
<img src="image.jpg" loading="lazy" alt="..." width="300" height="200">
```
2. через IntersectionObserver
```js
// HTML
// <img data-src="image.jpg" class="lazy" alt="..." width="300" height="200">
const lazyImages = [...document.querySelectorAll("img.lazy")];

const imageIntersectionObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      let lazyImage = entry.target;
      lazyImage.src = lazyImage.dataset.src; // Загрузка
      lazyImage.classList.remove("lazy");
      lazyImageObserver.unobserve(lazyImage); 
    }
  })

  lazyImages.forEach(image => imageIntersectionObserver.observe(image))
})
```

# Чем отличаются события input и change?

input - срабатывает на ввод данных в поле (т.е на каждое нажатие клавиши, при этом в отличие от событий клавиатуры, оно работает при любых изменениях значений, даже если они не связаны с клавиатурными действиями: вставка с помощью мыши или распознавание речи при диктовке текста)

change - срабатывает когда значение элемента изменилось (т.е. когда ввод окончен и фокус с элемента смещен)