# Модели параметров cookie

Если у вас есть группа **cookies**, которые связаны между собой, вы можете создать **Pydantic-модель** для их объявления. 🍪

Это позволит вам **переиспользовать модель** в **разных местах**, а также объявить проверки и метаданные сразу для всех параметров. 😎

/// note | Заметка

Этот функционал доступен с версии `0.115.0`. 🤓

///

/// tip | Совет

Такой же подход применяется для `Query`, `Cookie`, и `Header`. 😎

///

## Pydantic-модель для cookies

Объявите параметры **cookie**, которые вам нужны, в **Pydantic-модели**, а затем объявите параметр как `Cookie`:

{* ../../docs_src/cookie_param_models/tutorial001_an_py310.py hl[9:12,16] *}

**FastAPI** **извлечёт** данные для **каждого поля** из **cookies**, полученных в запросе, и выдаст вам объявленную Pydantic-модель.

## Проверка сгенерированной документации

Вы можете посмотреть объявленные cookies в графическом интерфейсе Документации по пути `/docs`:

<div class="screenshot">
<img src="/img/tutorial/cookie-param-models/image01.png">
</div>

/// info | Дополнительная информация

Имейте в виду, что, поскольку **браузеры обрабатывают cookies** особым образом и под капотом, они **не** позволят **JavaScript** легко получить доступ к ним.

Если вы перейдёте к **графическому интерфейсу документации API** по пути `/docs`, то сможете увидеть **документацию** по cookies для ваших *операций путей*.

Но даже если вы **заполните данные** и нажмёте "Execute", поскольку графический интерфейс Документации работает с **JavaScript**, cookies не будут отправлены, и вы увидите сообщение об **ошибке** как будто не указывали никаких значений.

///

## Запрет дополнительных cookies

В некоторых случаях (не особо часто встречающихся) вам может понадобиться **ограничить** cookies, которые вы хотите получать.

Теперь ваш API сам решает, <abbr title="Это шутка, на всякий случай. Это не имеет никакого отношения к согласию на использование cookie, но забавно, что даже API теперь может отклонять несчастные cookies. Съешьте печеньку. 🍪">принимать ли cookies</abbr>. 🤪🍪

Вы можете сконфигурировать Pydantic-модель так, чтобы запретить (`forbid`) любые дополнительные (`extra`) поля:

{* ../../docs_src/cookie_param_models/tutorial002_an_py39.py hl[10] *}

Если клиент попробует отправить **дополнительные cookies**, то в ответ он получит **ошибку**.

Бедные баннеры cookies, они всеми силами пытаются получить ваше согласие — и всё ради того, чтобы <abbr title="Это ещё одна шутка. Не обращайте на меня внимания. Выпейте кофе со своей печенькой. ☕">API его отклонил</abbr>. 🍪

Например, если клиент попытается отправить cookie `santa_tracker` со значением `good-list-please`, то в ответ он получит **ошибку**, сообщающую ему, что cookie `santa_tracker` <abbr title="Санта не одобряет пропажу печенья. 🎅 Ладно, больше никаких шуток про печенье.">не разрешён</abbr>:

```json
{
    "detail": [
        {
            "type": "extra_forbidden",
            "loc": ["cookie", "santa_tracker"],
            "msg": "Extra inputs are not permitted",
            "input": "good-list-please"
        }
    ]
}
```

## Заключение

Вы можете использовать **Pydantic-модели** для объявления <abbr title="Съешьте последнюю печеньку, прежде чем уйти. 🍪">**cookies**</abbr> в **FastAPI**. 😎
