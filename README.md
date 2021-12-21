1) Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea?

Команда:
git show -s --format="%H - %s" aefea
Ответ:
aefead2207ef7e2aa5dc81a34aedf0cad4c32545 - Update CHANGELOG.md

2) Какому тегу соответствует коммит 85024d3?

Команда:
git show -s  --format="%H -  %(describe)" 85024d3
Ответ:
85024d3100126de36331c6982bfaac02cdab9e76 -  v0.12.23

3) Сколько родителей у коммита b8d720? Напишите их хеши.

Команда:
git show --format="%P" b8d720
Ответ:
56cd7859e05c36c06b56d013b55a252d0bb7e158 9ea88f22fc6269854151c571162c5bcf958bee2b

Команда:
git log  -1 --format="%P" b8d720
Ответ:
56cd7859e05c36c06b56d013b55a252d0bb7e158 9ea88f22fc6269854151c571162c5bcf958bee2b

4)Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

Команда:
git log  v0.12.23..v0.12.24 --oneline
Ответ:
33ff1c03b (tag: v0.12.24) v0.12.24
b14b74c49 [Website] vmc provider links
3f235065b Update CHANGELOG.md
6ae64e247 registry: Fix panic when server is unreachable
5c619ca1b website: Remove links to the getting started guide's old location
06275647e Update CHANGELOG.md
d5f9411f5 command: Fix bug when using terraform login on Windows
4b6d06cc5 Update CHANGELOG.md
dd01a3507 Update CHANGELOG.md
225466bc3 Cleanup after v0.12.23 release

Команда:
git log --format="%H -%s" v0.12.23..v0.12.24
Ответ:
33ff1c03bb960b332be3af2e333462dde88b279e -v0.12.24
b14b74c4939dcab573326f4e3ee2a62e23e12f89 -[Website] vmc provider links
3f235065b9347a758efadc92295b540ee0a5e26e -Update CHANGELOG.md
6ae64e247b332925b872447e9ce869657281c2bf -registry: Fix panic when server is unreachable
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 -website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 -Update CHANGELOG.md
d5f9411f5108260320064349b757f55c09bc4b80 -command: Fix bug when using terraform login on Windows
4b6d06cc5dcb78af637bbb19c198faff37a066ed -Update CHANGELOG.md
dd01a35078f040ca984cdd349f18d0b67e486c35 -Update CHANGELOG.md
225466bc3e5f35baa5d07197bbc079345b77525e -Cleanup after v0.12.23 release

5)Найдите коммит в котором была создана функция func providerSource, ее определение в коде выглядит так func providerSource(...) (вместо троеточего перечислены аргументы).

Команда:
git log -S "func providerSource" --oneline
Ответ:
5af1e6234 main: Honor explicit provider_installation CLI config when present
8c928e835 main: Consult local directories as potential mirrors of providers

6) Найдите все коммиты в которых была изменена функция globalPluginDirs

Найдем файл где описана данна функция:
Команда:
git grep -p "globalPluginDirs("
Результат:
commands.go=func initCommands(
commands.go:            GlobalPluginDirs: globalPluginDirs(),
commands.go=func credentialsSource(config *cliconfig.Config) (auth.CredentialsSource, error) {
commands.go:    helperPlugins := pluginDiscovery.FindPlugins("credentials", globalPluginDirs())
plugins.go=import (
plugins.go:func globalPluginDirs() []string {

Подходит только plugins.go

Ищем коммиты с изменениями данной функции

Команда:
git log -L :globalPluginDirs:plugins.go -s --oneline
Результат:
78b122055 Remove config.go and update things using its aliases
52dbf9483 keep .terraform.d/plugins for discovery
41ab0aef7 Add missing OS_ARCH dir to global plugin paths
66ebff90c move some more plugin search path logic to command
8364383c3 Push plugin discovery down into command package

7) Кто автор функции synchronizedWriters
Команда:
git log -S "func synchronizedWriters" --format="%an -%aD -%h"
Результат:
James Bardin -Mon, 30 Nov 2020 18:02:04 -0500 -bdfea50cc
Martin Atkins -Wed, 3 May 2017 16:25:41 -0700 -5ac311e2a

Автор - Martin Atkins