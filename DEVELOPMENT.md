## Development Guidelines
В данном разделе описаны поддерживаемый процесс разработки, критерии приемки задач.

### Git flow
#### Организация веток
Все релизы версий спецификации всегда находится в ветке `master`. При внесении обратно совместимых изменений должна 
быть создана отдельная ветка с наименованием по следующему формату: `v<N>-add-new-feature`, где N - номер версии, в 
которую вносятся изменения, а `add-new-feature` - краткое описание изменения на английском языке.
При необходимости внесения обратно несовместимых изменений необходимо следовать правилам заведения новой версии 
спецификации. 

Разработка новой версии модели ведется в ветке 
`v<N>`, где N - номер версии. Как только версия готова к релизу, ветка заливается в ветку `master`. Все изменения, 
которые выполняются для версии N должны быть реализованы как feature-brach от ветки `v<N>`.

```
# перейти в ветку v1.0.0
git checkout v1.0.0
# скачать последние изменения
git pull
# создать новую ветку
git checkout -b v1.0.0-upgrade-profiles
```

#### Процесс внесения изменений
Для каждого изменения должна быть заведена задача в 
[GitHub Issues](https://github.com/dbratsun/windows-specification-ru/issues). 
Если задача взята в работу, нужно назначить пользователя в Assignee.

Каждая задача выполняется в отдельной ветке, которая должна быть создана от соответствующей основной ветки (либо от 
`master`, либо от `v<N>`). 

В каждой ветке должно быть минимально необходимое количество коммитов (в идеальном варианте изначально должен быть только один). 
Комментарий к коммиту должен быть в формате "Fixed #123 - 
add new 
feature", где #123 - ссылка на номер issue, к которой относится данное изменение. 

Как только изменение готово, можно пушить ветку в репозиторий.

Пример внесения изменений: 
```
git checkout v1.0.0
git pull
git checkout -b v1.0.0-upgrade-profile
# вносим изменения
git add -A
git commit -m "Fixed #2 - upgrade profile structure for dictionary"
git push -u origin v1.0.0-upgrade-profile
```
После этого нужно оформить [pull request](https://github.com/dbratsun/windows-specification-ru/compare) для этой задачи. 
В качестве _base_ ветки выбираем master или ветку версии, а в качестве _compare_ ветки выбираем только что запушенную ветку. 
После чего нажимаем _Create pull request_. В созданном pull request необходимо выбрать одного или несколько 
пользователей в качестве Reviewers. 

Если ревьюер в процессе проверки изменений оставляет комментарии, по результату которых необходимо внести изменения, 
то такие изменения оформляются в отдельном коммите (комментарий коммита может быть в свободной форме).

При отсутствии других замечаний ревьюер заливает изменений и закрывает задачу. 

В случае, если после выбора веток (или после создания pull request) высвечивается _Can’t automatically merge_, 
понадобится внести изменения в свою ветку. 
Такая ситуация может случиться, если несколько коммитеров затронули своими изменениями одно и то же место в файле и 
git не смог автоматически смерджить эти изменения. В случае подобных конфликтов необходимо сделать rebase текущей 
ветки на последнее изменение base ветки.

Пример rebase операции на ветке-версии:
```
# проверяем, что мы в compare ветке и нет ли незакоммиченных изменений. 
git status
# при необходимости добавляем изменения, здесь комментарий может быть уже в свободной форме
git add -A 
git commit -m "fix profile field description"
# скачиваем изменения удаленного репозитория
git fetch
# проводим rebase 
git rebase origin/v.1.0.0
# при конфликтах процесс остановится и будет предложено внести исправления вручную
# проверяем в каких файлах конфликты
git status
# исправляем конфликты, далее добавляем исправленные файлы и продолжаем rebase
git add -A 
git rebase --continue
# пушим изменения в удаленный репозиторий
git push --force
```
В случае необходимости прервать rebase в процессе ручного исправления конфликтов, можно выполнить `git rebase --abort`.

### Definition of Done
В данном разделе описаны требования к вносимым изменениям, которые должны быть соблюдены для успешной заливки в 
основную ветку.
- Pull request просмотрен и одобрен минимум одним ревьюером
- Добавлена документация 
#### Acceptance criteria. Описание модели
- каждое поле содержит description тэг, который содержит вкратце определение и описывает общие правила для данного поля
- для yaml модели обновлен файл json схемы, а также примеры 

