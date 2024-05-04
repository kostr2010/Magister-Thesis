**Narrative**

**Problem statement**

We have a multilanguage platform with a very reusable compiler (explain runtime interface). This platform already has experience reusing compiler to make bytecode optimizer (explain usage of runtime interface, compare to ART's d8, which is a separate compiler instead of the extension of the existing one). Users have a requirement to be able to manipulate existing binary files to alter program's behaviour after it has already been built.

**Task**

To satisfy user requirements, it is proposed to develop API on top of the existing compiler API that will provide users with the capability to inspect and modify instructions. It is also proposed to provide capabilities to inspect and modify metadata of the binary file, like properties of functions and types. Said API needs to be able to work with the binary files obtained from frontends from different languages without exposing implementation details of any of frontends as much as possible.

**Why to use compiler?**

Current compiler provides us with the SSA form of the bytecode. This form gives user DFG and CFG "out of the box" and enables user to write many classic analyses and optimisation passes. Using SSA form also removes the need for user to be concerned with the regalloc problems (explain that Java has stack isa and thus does not have to worry about such things. We have register based isa and for us it is critical). Building on top of the existing compiler also gives us many service functionality out of the box: different passes that were already implemented for internal use can be make public and reused for user code.

**Library design**

KN: metadata API + multilang (if done).
IT: IR API + implementation decisions.

**Library implementation**

**User examples**

Show how API solves external users problems.

1. Введение
   - Как происходит исполнение кода программ? С помощью исполнения инструкций (ассемблер или байткод) и работы с метаданными, которые находятся внутри заданного файл формата (упомянуть про elf и байткод файл формат)
   - Над промежуточным представлением и метаданными иногда нужно производить операции вручную: рассказать про каждый кейс, зачем это может быть нужно (профайлинг, инструментация, аннотации, AOP и тд). Сделать акцент на умении писать оптимизации над программами
   - сказать, что хотим иметь один интерфейс для манипуляции над несколькими байткода (или сущностями из нескольких ЯП). Тут можно немного затронуть тему интеропа и зачем вообще нужен интероп
2. Цели и задачи
   - подчеркнуть, что в данной работе речь идет о байткоде и файл формате для ВМки, а не native. А точнее речь пойдет в основном именно про манипуляцию над самим байткодом
   - исходя из написанных выше требований, сформулировать цель и задачи работы
3. Обзор существующих решений
   - упомянуть о наличии множества аналогов для джавы, но акцентировать внмиание на ASM и javassist
   - расписать ASM
   - расписать javassist

------------------ Это всё должно занять страниц 10-15 ------------------

4. Исследование и построение решения задачи
   - Напомнить еще раз, что манипулировать можно двумя видами сущностей в файл формате: байткод и метадата. И что в данной статье пойдет речь про байткод. Показать общую схему решения вместе с метадатой и abc2program
   - Рассказ про 2 исы: статическую и динамическую
   - Предлагается манипулировать не самим байткодом, а компиляторным IR в SSA форме. Сказать, почему именно IR в SSA форме
   - Общая схема решения (байткод -> SSA IR -> байткод)
   - Рассказ про SSA форму и компиляторный граф. Как технически устроен компиляторный граф (стартовые базовые блоки, циклы, ифы и тд)
   - Построение SSA формы из байткода
   - Рассказать про идею phi <-> X_Var и описать алгоритм конвертации в обе стороны
   - Построение байткода из SSA формы: регаллок, кодген
   - Аллокация инлайн кэш слотов. Рассказ про инлайн кэши в целом
   - Описание самого API: апи для графа, базового блока, инструкций
   - Некоторые нетривиальные апи: обход в RPO и других порядках, построение дерева доминаторов, вставка try-catch конструкции, вставка loop конструкции
   - Можно описать аннотации, чтобы добить объем
5. Примеры использования
   - логгирование
   - branch elimination
6. Заключение
   - подытожить сделанное
   - рассказать о том, что предстоит сделать в дальнейшем
7. Литература

------------------ Это всё должно занять страниц 40-45 ------------------
