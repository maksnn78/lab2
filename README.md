# Лабораторная работа №2
## Аксентьев Максим ИУ8-22
## Часть 1
```bash
git clone https://github.com/maksnn78/lab2.git
cd lab2
```
### Создание начальной версии
```bash
nano hello_world.cpp
```
```cpp
#include <iostream>
using namespace std;

int main() {
    cout << "Hello world" << endl;
    return 0;
}
```
### Коммит
```bash
git add hello_world.cpp 
git commit -m "initial commit: hello world"
git push origin main
```
### Изменение программы (ввод имени)
```bash
nano hello_world.cpp
```
```cpp
#include <iostream>
using namespace std;

int main() {
    string name;
    cin >> name;
    cout << "Hello world from " << name << endl;
    return 0;
}
```
### Коммит
```bash
git add hello_world.cpp
git commit -m "add user input"
git push origin main
```
## Часть 2
```bash
git checkout -b patch1
git branch
```
### Изменение кода (убрать using namespace)
```bash
nano hello_world.cpp
```
```cpp
#include <iostream>

int main() {
    std::string name;
    std::cin >> name;
    std::cout << "Hello world from " << name << std::endl;
    return 0;
}
```
### Коммит
```bash
git add hello_world.cpp
git commit -m "fix style: remove using namespace"
git push origin patch1
```
### добавить комментарии в код (в ветке patch1)
```bash
nano hello_world.cpp
```
```cpp
#include <iostream>

// Program prints greeting message with user input
int main() {
    std::string name;
    std::cin >> name;
    // output greeting
    std::cout << "Hello world from " << name << std::endl;
    return 0;
}
```
### Коммит
```bash
git add hello_world.cpp
git commit -m "add comments"
git push origin patch1
```
### Слияние Pull Request
Pull Request patch1 → main был объединён (merge) на GitHub.
Ветка patch1 была удалена после слияния.
### Обновление локального репозитория
```bash
git checkout main
git pull origin main
git branch -d patch1
```
### История коммитов:
```bash
git log --oneline --graph --all
```
```bash
* f6ed42e (HEAD -> main, origin/main) Merge pull request #1 from maksnn78/patch1
|\
| * ee4457a (origin/patch1) add comments
| * 83cdd44 fix style: remove using namespace
|/
* 3fa42c3 add user input
* 4d3559d Update hello_world.cpp
* 512c0c0 initial commit: hello world
```
## Часть 3
```bash
git checkout -b patch2
sudo apt update
sudo apt install clang-format
```
### Форматирование кода
```bash
clang-format -i -style=Mozilla hello_world.cpp
```
### Коммит
```bash
git add hello_world.cpp
git commit -m "format code with clang-format"
git push origin patch2
```
### Создание конфликта
В репозитории специально изменил комментарий, добавив знаки препинания
```bash
git pull --rebase origin main
```
```bash
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 957 bytes | 191.00 KiB/s, done.
From https://github.com/maksnn78/lab2
 * branch            main       -> FETCH_HEAD
   f6ed42e..34a7a7c  main       -> origin/main
Auto-merging hello_world.cpp
CONFLICT (content): Merge conflict in hello_world.cpp
error: could not apply c4a88d0... format code with clang-format
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply c4a88d0... format code with clang-format
```
### Исправление конфликта
```bash
nano hello_world.cpp
```
```cpp
#include <iostream>

<<<<<<< HEAD
// Program prints,,,,, greeting message with user input
int main() {
    std::string name;
    std::cin >> name;
    // output greeting
    std::cout << "Hello world from " << name << std::endl;
    return 0;
=======
// Program prints greeting message with user input
int
main()
{
  std::string name;
  std::cin >> name;
  // output greeting
  std::cout << "Hello world from " << name << std::endl;
  return 0;
  >>>>>>> c4a88d0 (format code with clang-format)
}
```
Изменим на
```cpp
#include <iostream>

// Program prints greeting message with user input
int main() {
    std::string name;
    std::cin >> name;
    // output greeting
    std::cout << "Hello world from " << name << std::endl;
    return 0;
}
```
### Завершение rebase
```bash
git add hello_world.cpp
git rebase --continue
git push origin patch2 --force
```
Добавление файла в индекс означает, что конфликт был решён.
Команда rebase --continue продолжает процесс переноса коммитов.
Force push используется, так как история была изменена после rebase.

### Слияние изменений (patch2 → main)
Pull Request был успешно объединён с основной веткой.
Ветка patch2 была удалена после слияния.
### Обновление локального репозитория
```bash
git checkout main
git pull origin main
git branch -d patch2
```
