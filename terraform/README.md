# Файлы которые будут проигнорированны благодаря добавленному .gitignore:
1.	файлы содержащиеся в локальном каталоге .terraform
2.	файлы состояния .tfstate files
3.	файлы .tfvars, которые могут содержать важные данные, такие как пароль, закрытые ключи и другие секреты. Они не должны быть частью системы контроля версий, поскольку представляют собой точки данных, которые потенциально чувствительны и могут изменяться в зависимости от среды.
4.	журналы сбоев
5.	файлы переопределения
6.	файлы конфигурации CLI
