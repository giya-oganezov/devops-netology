# Домашнее задание к занятию «2.4. Инструменты Git»

1.  Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.

	Указали git show aefea и получили следующий вывод:    
	
		commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
		Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
		Date: Thu Jun 18 10:29:58 2020 -0400
		Update CHANGELOG.md
2. Какому тегу соответствует коммит `85024d3`?
Коммит `85024d3` соответствует тэгу v0.12.23, это значенние получили использовав команду git show 85024d3
3. Сколько родителей у коммита `b8d720`? Напишите их хеши.
Запускаем Git show b8d720, вывод указывает на то что это merge COMMIT и он имеет два родительских commits: 1) 56cd7859e 2) 9ea88f22f
4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.    
	Способ решения: git log v0.12.23..v0.12.24 --pretty=format:'%h %s'
	
		33ff1c03b v0.12.24
		b14b74c49 [Website] vmc provider links
		3f235065b Update CHANGELOG.md
		6ae64e247 registry: Fix panic when server is unreachable
		5c619ca1b website: Remove links to the getting started guide's old location
		06275647e Update CHANGELOG.md
		d5f9411f5 command: Fix bug when using terraform login on Windows
		4b6d06cc5 Update CHANGELOG.md
		dd01a3507 Update CHANGELOG.md
		225466bc3 Cleanup after v0.12.23 release
5. Найдите коммит в котором была создана функция `func providerSource`, ее определение в коде выглядит так `func providerSource(...)` (вместо троеточего перечислены аргументы).

	Находим файл с функцией выполнив: git grep 'func providerSource'  имя файла provider_source.go. проследим историю функции, далее выполняем git log -L :providerSource:provider_source.go  и получаем:
	
		commit 8c928e83589d90a031f811fae52a81be7153e82f
		Author: Martin Atkins <mart@degeneration.co.uk>
		Date: Thu Apr 2 18:04:39 2020 -0700
6. Найдите все коммиты в которых была изменена функция `globalPluginDirs`.
       

	Выполняем git grep -p 'globalPluginDirs' чтобы определить файл с функцией, из вывода получаем файл plugins.go, далее используем
	git log -L :globalPluginDirs:plugins.go и получаем список коммитов в которых была изменена функция `globalPluginDirs`.
	
		commit 5af1e6234ab6da412fb8637393c5a17a1b293663
		commit 92d6a30bb4e8fbad0968a9915c6d90435a4a08f6
		commit 8c928e83589d90a031f811fae52a81be7153e82f

7.  Кто автор функции `synchronizedWriters`?
	Выполняем git log -S 'func synchronizedWriters', ответ:
	
		Author: Martin Atkins 