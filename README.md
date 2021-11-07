# Домашнее задание к занятию «2.4. Инструменты Git»

1. ###Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`
   - `aefead2207ef7e2aa5dc81a34aedf0cad4c32545` `Update CHANGELOG.md` 
- Команда: `git log -1 --pretty=oneline aefea`
 
2. ###Какому тегу соответствует коммит 85024d3
   - `v0.12.23`
- Команда: `git name-rev --tags --name-only 85024d3`
 
3. ###Сколько родителей у коммита b8d720? Напишите их хеши.
   - У коммита `b8d720` два родителя
   - `56cd7859e05c36c06b56d013b55a252d0bb7e158`
   - `9ea88f22fc6269854151c571162c5bcf958bee2b`
- Команда: `git rev-parse b8d720^@`
 
4. ###Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

   - `33ff1c03bb960b332be3af2e333462dde88b279e` `(tag: v0.12.24) v0.12.24`
   - `b14b74c4939dcab573326f4e3ee2a62e23e12f89` `[Website] vmc provider links`
   - `3f235065b9347a758efadc92295b540ee0a5e26e` `Update CHANGELOG.md`
   - `6ae64e247b332925b872447e9ce869657281c2bf` `registry: Fix panic when server is unreachable`
   - `5c619ca1baf2e21a155fcdb4c264cc9e24a2a353` `website: Remove links to the getting started guide's old location`
   - `06275647e2b53d97d4f0a19a0fec11f6d69820b5` `Update CHANGELOG.md`
   - `d5f9411f5108260320064349b757f55c09bc4b80` `command: Fix bug when using terraform login on Windows`
   - `4b6d06cc5dcb78af637bbb19c198faff37a066ed` `Update CHANGELOG.md`
   - `dd01a35078f040ca984cdd349f18d0b67e486c35` `Update CHANGELOG.md`
   - `225466bc3e5f35baa5d07197bbc079345b77525e` `Cleanup after v0.12.23 release`

- Команда: `git log --pretty=oneline v0.12.23...v0.12.24`

5. ###Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточия перечислены аргументы).

   - `8c928e835`
- Команда: `git log --pretty=format:%h -S"func providerSource("`
 
6. ###Найдите все коммиты в которых была изменена функция globalPluginDirs
   - `fcdb5d2e5`
   - `22a2580e9`
   - `35a058fb3`
   - `c0b176109`
   - `8364383c3`
- Команда:`git log -G globalPluginDirs --source --all --pretty=format:%h`

7. ###Кто автор функции synchronizedWriters
   - `Martin Atkins`
- Комманда:`git log -S synchronizedWriters --pretty=format:%an --reverse | head -1`