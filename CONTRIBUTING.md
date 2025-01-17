# Contributing guide

## Environment setup

1. [установить poetry](https://python-poetry.org/docs/#installation) (менеджер пакетов):
   после этого возможно нужно будет добавить в ~/.bashrc строчку export
   PATH="$HOME/.poetry/bin:$PATH" в итоге у вас должна успешно выполниться команда poetry
   --version и напечатать что-то вроде Poetry version 1.1.4
1. отключить автоматическое создание env-ов (мы используем conda): poetry config
   virtualenvs.create false
1. склонировать репо ml-mipt
1. создать новый conda environment: conda create -n ml-mipt python=3.8
1. активировать env: conda activate ml-mipt
1. находясь в папке репозитория ввести команду установки пакетов: poetry install
   --remove-untracked
1. активировать git hooks: pre-commit install
1. сделать новую ветку от master с названием вашей папки: git checkout -b <your week name>
1. запустить pre-commit на вашей папке: pre-commit run --files week0_01_intro_and_kNN/\*
1. просмотреть все файлы на адекватность полученных изменений (это самая важная часть
   работы) и исправть места, где:

   - ругается flake8 (не использованные импотры, расположение импортов не в начале и проч)
   - black делает что-то лишнее. Например, если есть длинный захардкоженный лист или дикт,
     то он его развернёт в очень длинный столбик - в таком случае можно сказать black-у
     игнорировать этот кусок кода:

     ```python
     this_is_formatted_code('ok')

     # fmt: off
     long_hardcoded_list = [1, 2, 3, 4, 5, 6, 12451, 4513451345, 34513451345]
     result = "it won't be formatted"
     # fmt: on

     here = "formatting returns"
     ```

   - если будут вопросы по каким-то ошибкам/руганиям тулзов - пишите в чат

1. делаете pull request в master, я его проверяю и мерджу

None: pre-commit не позволит вам сделать комммит пока не будут исправлены все ошибки
хуков - это так и задумано для поддержания кхорошего уровня кода. Но в нашем случае вы
можете не знать как пофиксить все проблемы, в этом случае есть возможность сделать коммит,
игнорируя хуки: git commit -nv -m 'broken commit'. Как правило это нужно, чтобы предметно
задать вопрос по получившемуся коду (я могу посмотреть этот коммит у себя, напимер). В
конечном счёте вмерджены будут только те ветки, в которых будут справлены все ошибки.

## Refactoring

- Места для написания собственного кода нужно помечать `# YOUR CODE HERE: description`.
  Также в этих местах должен быть синтаксически верный Python код - то есть вам нужно
  присвоить переменным какие-то значения (лучше всего подходит `None`)
- Назначать адекватные имена всем переменным. В частности необходимо переназвать
  однобуквенные переменные в стиле `a = <some code initializing action>` в говорящие имена
  `action = <same code>`
- Перед коммитом нужно оставить клетки, которые могут быть запущены без ошибок, с их
  нормальным выходом. При этом не оставлять в качестве выхода клетки `Exception`. Клетки,
  требующие кода нужно оставить с пустым выходом
- Если код семинара производит какие-либо новые файлы/папки в этой папке, то создайте файл
  `.gitignore`, в который запишите эти файлы/папки, чтобы они не были случайно добавлены в
  `git` в будущем.
- Все данные (датасеты) нужно вынести в папку `/datasets` (которая расположена на верхнем
  уровне)
- Нужно проверить, что ссылки на `collab` в `README.md` вашей папки работают корректно

## Code quality rules

- new code is not duplicate of existing code. If so, rethink your design (move duplicating
  code to function or class and use it several times).
- all functions have annotations e.g.
  `def train_model(model: nn.Module, epochs: int = 10)`
- new elements (functions, classes, params) have their docstrings
- changed elements' docstrings are relevant. In other words current behaviour described,
  not previous
- no _one letter variables_ are introduced (e.g. `x`, `y` for data, although `i` for
  simple counter is ok) as well as _very long variables_ (above 15 characters)
