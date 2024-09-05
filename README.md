# Задание 1 (Уровень 1 и 2)
#### Разделение на компоненты
В результате анализа коды были выделены следующие микро-фронтенды:
* **host-app** 
    Является главным приложением. Определяет расположение других микро-фронтендов на странице, содержит логику маршрутизации, определяет правила взаимодействия микро-фронтендов, загружает другие микро-фронтэнды.
* **lib-mf**
    Содержит общие копоненты, другие микро-фронтенды может переиспользовать, а данном случае это комонент PopupWithForm.
* **cards-mf**
    Содержит список карточек, возможность добавления новых каточек, удаление карточек, отображение лайков и возможность поставить лайк.
* **profile-mf**
    Содержит компоненты для отображения информации о пользвателе, изменения аватара и описания.
* **auth-mf**
    Содержит форму регистрации и форму входа.

#### Выбор технологии микро-фронтенда

В результаты анализа информации о SingleSPA и Webpack Federation Plugin были сделаны следующие выводы:
* SingleSPA является полноценным фреймворком для интеграции микро-фронтэндов, имеет решения для типичных проблемм, в то время как Webpack Federation Plugin просто предоставляет возможность выгружать части другого приложения.
* SingleSPA имеет решения для интеграции приложений написанных на разных фремворках React, Angular, Vue, в то время как для с ипользованием Webpack Federation Plugin придётся писать много низкоуровневого кода.
* Webpack Federation Plugin прост в использовании если интегрируются приложения написанные на одном и том же фреймворке, например React.
* Webpack имеет очень большие комьюнити людей кто его поддерживает, что же касается SingleSPA они не имеют крупных спонсоров и живут в том числе на донаты, есть риск что последний может заморожен

Вцелом, оба фреймворка имеют значительные минусы, в данном конкретном случае выбор пал на **Webpack Federation Plugin**, потому что всё приложение написано на React и его проще разделить используя этот подход.

#### Структура проекта:

Были произведены следующие действия:
* созданы заготовки проектов через npx create-mf-app
* все проекты имеют такую же структуру как и оригинальный
* в папку components перенесены компоненты 
* папка block разделены на части, соответствующие микро-фронтендам и скопирована в них
* папка mages разделена по микро-фронтэндам
* auth.js перенесён в auth-mf
* api.js разделён и перенесён в cards-mf и profile-mf
* настроены expose и remote

```
.
├── auth-mf
│   ├── compilation.config.js
│   ├── package-lock.json
│   ├── package.json
│   ├── src
│   │   ├── App.jsx
│   │   ├── blocks
│   │   │   ├── auth-form
│   │   │   │   ├── __button
│   │   │   │   │   └── auth-form__button.css
│   │   │   │   ├── __form
│   │   │   │   │   └── auth-form__form.css
│   │   │   │   ├── __input
│   │   │   │   │   └── auth-form__input.css
│   │   │   │   ├── __link
│   │   │   │   │   └── auth-form__link.css
│   │   │   │   ├── __text
│   │   │   │   │   └── auth-form__text.css
│   │   │   │   ├── __textfield
│   │   │   │   │   └── auth-form__textfield.css
│   │   │   │   ├── __title
│   │   │   │   │   └── auth-form__title.css
│   │   │   │   └── auth-form.css
│   │   │   └── login
│   │   │       └── login.css
│   │   ├── components
│   │   │   ├── Login.js
│   │   │   └── Register.js
│   │   ├── contexts
│   │   │   └── CurrentUserContext.js
│   │   ├── index.css
│   │   ├── index.html
│   │   ├── index.js
│   │   └── utils
│   │       └── auth.js
│   └── webpack.config.js
├── cards-mf
│   ├── compilation.config.js
│   ├── package-lock.json
│   ├── package.json
│   ├── src
│   │   ├── App.jsx
│   │   ├── blocks
│   │   │   ├── card
│   │   │   │   ├── __delete-button
│   │   │   │   │   ├── _hidden
│   │   │   │   │   │   └── card__delete-button_hidden.css
│   │   │   │   │   ├── _visible
│   │   │   │   │   │   └── card__delete-button_visible.css
│   │   │   │   │   └── card__delete-button.css
│   │   │   │   ├── __description
│   │   │   │   │   └── card__description.css
│   │   │   │   ├── __image
│   │   │   │   │   └── card__image.css
│   │   │   │   ├── __like-button
│   │   │   │   │   ├── _is-active
│   │   │   │   │   │   └── card__like-button_is-active.css
│   │   │   │   │   └── card__like-button.css
│   │   │   │   ├── __like-count
│   │   │   │   │   └── card__like-count.css
│   │   │   │   ├── __title
│   │   │   │   │   └── card__title.css
│   │   │   │   └── card.css
│   │   │   └── places
│   │   │       ├── __item
│   │   │       │   └── places__item.css
│   │   │       ├── __list
│   │   │       │   └── places__list.css
│   │   │       └── places.css
│   │   ├── components
│   │   │   ├── AddPlacePopup.js
│   │   │   └── Card.js
│   │   ├── images
│   │   │   ├── delete-icon.svg
│   │   │   ├── like-active.svg
│   │   │   └── like-inactive.svg
│   │   ├── index.css
│   │   ├── index.html
│   │   ├── index.js
│   │   └── utils
│   │       └── api.js
│   └── webpack.config.js
├── host-app
│   ├── compilation.config.js
│   ├── package-lock.json
│   ├── package.json
│   ├── src
│   │   ├── App.jsx
│   │   ├── blocks
│   │   │   ├── content
│   │   │   │   └── content.css
│   │   │   ├── footer
│   │   │   │   ├── __copyright
│   │   │   │   │   └── footer__copyright.css
│   │   │   │   └── footer.css
│   │   │   ├── header
│   │   │   │   ├── __auth-link
│   │   │   │   │   └── header__auth-link.css
│   │   │   │   ├── __logo
│   │   │   │   │   └── header__logo.css
│   │   │   │   ├── __logout
│   │   │   │   │   └── header__logout.css
│   │   │   │   ├── __user
│   │   │   │   │   └── header__user.css
│   │   │   │   ├── __wrapper
│   │   │   │   │   └── header__wrapper.css
│   │   │   │   └── header.css
│   │   │   └── page
│   │   │       ├── __content
│   │   │       │   └── page__content.css
│   │   │       ├── __section
│   │   │       │   └── page__section.css
│   │   │       └── page.css
│   │   ├── bootstrap.js
│   │   ├── components
│   │   │   ├── App.js
│   │   │   ├── Footer.js
│   │   │   ├── Header.js
│   │   │   ├── ImagePopup.js
│   │   │   ├── InfoTooltip.js
│   │   │   ├── Main.js
│   │   │   └── ProtectedRoute.js
│   │   ├── images
│   │   │   ├── avatar.jpg
│   │   │   ├── card_1.jpg
│   │   │   ├── card_2.jpg
│   │   │   ├── card_3.jpg
│   │   │   ├── error-icon.svg
│   │   │   ├── logo.svg
│   │   │   └── success-icon.svg
│   │   ├── index.css
│   │   ├── index.html
│   │   ├── index.js
│   │   ├── serviceWorker.js
│   │   └── vendor
│   │       ├── fonts
│   │       │   ├── Inter-Black.woff2
│   │       │   └── Inter-Regular.woff2
│   │       ├── fonts.css
│   │       └── normalize.css
│   └── webpack.config.js
├── lib-mf
│   ├── compilation.config.js
│   ├── package-lock.json
│   ├── package.json
│   ├── src
│   │   ├── App.jsx
│   │   ├── blocks
│   │   │   └── popup
│   │   │       ├── __button
│   │   │       │   ├── _disabled
│   │   │       │   │   └── popup__button_disabled.css
│   │   │       │   └── popup__button.css
│   │   │       ├── __caption
│   │   │       │   └── popup__caption.css
│   │   │       ├── __close
│   │   │       │   └── popup__close.css
│   │   │       ├── __content
│   │   │       │   ├── _content
│   │   │       │   │   └── popup__content_content_image.css
│   │   │       │   └── popup__content.css
│   │   │       ├── __error
│   │   │       │   ├── _visible
│   │   │       │   │   └── popup__error_visible.css
│   │   │       │   └── popup__error.css
│   │   │       ├── __form
│   │   │       │   └── popup__form.css
│   │   │       ├── __icon
│   │   │       │   └── popup__icon.css
│   │   │       ├── __image
│   │   │       │   └── popup__image.css
│   │   │       ├── __input
│   │   │       │   ├── _type
│   │   │       │   │   └── popup__input_type_error.css
│   │   │       │   └── popup__input.css
│   │   │       ├── __label
│   │   │       │   └── popup__label.css
│   │   │       ├── __status-message
│   │   │       │   └── popup__status-message.css
│   │   │       ├── __title
│   │   │       │   └── popup__title.css
│   │   │       ├── _is-opened
│   │   │       │   └── popup_is-opened.css
│   │   │       ├── _type
│   │   │       │   ├── popup_type_edit-avatar.css
│   │   │       │   └── popup_type_remove-card.css
│   │   │       └── popup.css
│   │   ├── components
│   │   │   └── PopupWithForm.js
│   │   ├── images
│   │   │   └── close.svg
│   │   ├── index.css
│   │   ├── index.html
│   │   └── index.js
│   └── webpack.config.js
└── profile-mf
    ├── compilation.config.js
    ├── package-lock.json
    ├── package.json
    ├── src
    │   ├── App.jsx
    │   ├── blocks
    │   │   └── profile
    │   │       ├── __add-button
    │   │       │   └── profile__add-button.css
    │   │       ├── __description
    │   │       │   └── profile__description.css
    │   │       ├── __edit-button
    │   │       │   └── profile__edit-button.css
    │   │       ├── __image
    │   │       │   └── profile__image.css
    │   │       ├── __info
    │   │       │   └── profile__info.css
    │   │       ├── __title
    │   │       │   └── profile__title.css
    │   │       └── profile.css
    │   ├── components
    │   │   ├── EditAvatarPopup.js
    │   │   └── EditProfilePopup.js
    │   ├── images
    │   │   ├── add-icon.svg
    │   │   └── edit-icon.svg
    │   ├── index.css
    │   ├── index.html
    │   ├── index.js
    │   └── utils
    │       └── api.js
    └── webpack.config.js
```

---
# Задание 2
Решение: https://drive.google.com/file/d/1AozM-D_QuHqdoLjL95hPKzw7ZJkc1vQc/view?usp=sharing
