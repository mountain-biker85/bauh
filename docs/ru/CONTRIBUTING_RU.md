# Как я могу внести свой вклад?
Итак, вы можете...
* Сообщать об ошибках
* Добавлять улучшения
* Исправлять ошибки
* Добавить новый перевод или исправить существующие

# Сообщения об ошибках
Лучший способ сообщить об ошибках-следовать этим основным рекомендациям:

* Сначала опишите в названии трекера проблем, что пошло не так.
* В теле обрисуйте основную симптоматику того, что именно происходит, объясните, как вы получили ошибку шаг за шагом. Если вы включаете вывод сценария, убедитесь, что вы запускаете сценарий с флагом verbose `-v`.
* Объясните, что вы ожидали, что произойдет, и что произошло на самом деле.
* Дополнительно, если вы хотите, если вы программист, вы можете попробовать самостоятельно сделать Pull Request, который исправит проблему.

# Добавление улучшений
The way to go here is to ask yourself if the improvement would be useful for more than just a singular person, if it's for a certain use case then sure!

* В любом pull request подробно объясните, какие изменения вы внесли
* Объясните, почему вы считаете, что эти изменения могут быть полезны
* Если он исправляет ошибку, обязательно ссылайтесь на саму проблему.
* Следуйте стилю кода [PEP 8](https://www.python.org/dev/peps/pep-0008/), чтобы сохранить последовательность кода..

# Добавление нового перевода
* Чтобы добавить новый перевод, вам нужно будет создать файл в каждом каталоге, указанном ниже, с именем первых двух букв языка в формате ISO (например : для русского 'russian' было бы 'ru'):
- **bauh/view/resources/locale**
- **bauh/gems/appimage/resources/locale**
- **bauh/gems/arch/resources/locale**
- **bauh/gems/flatpak/resources/locale**
- **bauh/gems/snap/resources/locale**
