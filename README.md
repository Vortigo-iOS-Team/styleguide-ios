# iOS Styleguide

## Objetivos

- Facilitar o entendimento de novos membros
- Facilitar a manutenção do código
- Reduzir erros simples de desenvolvimento
- Manter as discussões dos PRs/MRs focadas na lógica e construção da tarefa

## Princípios
- Este documento é um complemento à [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/), as guides descritas aqui não devem contradizer as guidelines da Apple.
- Antes de submeter seu código, tenha em mente o comportamento para manter a indentação: <kbd>^</kbd> + <kbd>I</kbd>.
- Antes de submeter seu código, verifique se seu código não quebra nenhuma regra do Swift Lint.
- Exceções à estas regras devem ser justificadas.

## Styleguide 

### Nomenclatura

Todas as declarações devem ser auto-explicativas e consistir de palavras do Inglês Americano, além de formar frases legíveis e que façam sentido quando lidas.
Usar nomes descritivos torna o código mais simples de ler e entender. Esta styleguide é apenas uma extensão das diretrizes oficiais Swift API Design Guidelines e não deve contradizer o que consta no documento.

#### Um bom nome não deve precisar de comentários
Exemplos:
##### Não preferível
```swift
let countdown = 100 // Contagem para o natal (em dias)
```

##### Correto
```swift
let countdownForChristmasInDays = 100
```

#### Priorizar a clareza ao invés da brevidade
Exemplos:
##### Não preferível
```swift
let i =  0 // evitar palavras sem significado
let cat = "cat or category?" // evitar siglas
let url =  "www.some-url.com?" // qual url?
```

##### Correto
```swift
let count = 0
let theCat = "O gato"
let category = "Categoria"
let imageURL = "www.image-url.com"
```

#### Usar `UpperCamelCase` para types e protocols. Usar `lowerCamelCase` para o restante
Exemplos:
```swift
protocol Vehicle { }

final class Car: Vehicle {
	enum Brand {
		case ferrari
		case honda
		case hyundai
	}
    
	var brand: Brand = .honda
	var carName = "Any car name"
}

struct Vehicle { }

let myCar = Car()
```

#### Nomear booleans como isTrackable, hasMedicines, etc, para deixar claro que são booleans
Exemplos:
##### Não preferível
```swift
struct Product {
	let favorited = false
}
```

##### Correto
```swift
struct Product {
	let isFavorited = false
}
```

#### Usar nomes baseados no propósito e não no tipo
Exemplos:
##### Não preferível
```swift
let view = UIView()
let string = "any-text"
```

##### Correto
```swift
let footerView = UIView()
let fullName = "John Doe"
```

#### Em caso de ambiguidade, incluir uma dica sobre o tipo
Exemplos:
##### Não preferível
```swift
let title: UILabel
let confirm: UIButton
```

##### Correto
```swift
let titleLabel: UILabel 
let confirmButton: UIButton
```

#### Os nomes de `class/struct` devem ser um substantivo ou uma frase nominal. Preferencialmente não ser um verbo
Exemplos:
##### Não preferível
```swift
class Buy { }
```

##### Correto
```swift
class Purchase { }
```

#### Os nomes de funções de manipulação de eventos devem ser nomeados usando o `past-tense`
Exemplos:
##### Não preferível
```swift
func modelChanged() { }
func handleCancelButtonTap() { }
```

##### Correto
```swift
func modelDidChange() { }
func didTapCancelButton() { }
```

#### Evitar prefixos do `Objective-C`
Exemplos:
##### Não preferível
```swift
class VTLoginViewController { }
```

##### Correto
```swift
class LoginViewController { }
```

### Nomes dos Arquivos

#### O nome do arquivo deve conter o mesmo nome da classe, struct etc, que está sendo declarado dentro do arquivo
Exemplos:
##### Não preferível
`Models.swift`
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
}
```

##### Correto
`UserModel.swift`
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
}
```

#### Evitar declaração de vários objetos dentro do mesmo arquivo
Exemplos:
##### Não preferível
`Models.swift`
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
}

struct AddressModel {
    let country: String
    let city: String
    let number: Int
}
```

##### Correto
`UserModel.swift`
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
}
```
`AddressModel.swift`
```swift
struct AddressModel {
    let country: String
    let city: String
    let number: Int
}
```

#### Em casos em que uma `struct`/`class`/`enum` dependa de outra `struct`/`class`/`enum`, usar outro arquivo ou inserir dentro da própria `struct`/`class`/`enum` a declaração. 
Exemplos:
##### Não preferível
`Models.swift`
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
    let address: AddressModel
}

struct AddressModel {
    let country: String
    let city: String
    let number: Int
}
```

##### Correto
`UserModel.swift`
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
    let address: AddressModel
}
```
`AddressModel.swift`
```swift
struct AddressModel {
    let country: String
    let city: String
    let number: Int
}
```

ou

`UserModel.swift`
```swift
struct UserModel {
    struct AddressModel {
        let country: String
        let city: String
        let number: Int
    }

    let name: String
    let lastName: String
    let birthDate: String
    let address: AddressModel
}
```

ou

##### Correto
`UserModel.swift`
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
    let address: AddressModel
}
```
`AddressModel.swift` ou `UserModel+AddressModel.swift`
```swift
extension UserModel {
    struct AddressModel {
        let country: String
        let city: String
        let number: Int
    }
}
```

### Variáveis/Constantes

#### Alguns tipos de variáveis/constantes tem seu tipo reconhecido implicitamente, não é necessário adicionar tipagem.
Exemplos:
##### Não preferível
```swift
let name: String = "Name"
```

##### Correto
```swift
let name = "Name"
```

##### Não preferível
```swift
let quantity: Int = 2
```

##### Correto
```swift
let quantity = 2
```

##### Não preferível
```swift
let weight: Double = 60.0
```

##### Correto
```swift
let weight = 60.0
```

#### Uma variável/constante deve possuir um whitespace entre o `:` e o seu tipo
Exemplos:
##### Não preferível
```swift
let topConstraintValue:CGFloat = 24.0
```

##### Correto
```swift
let topConstraintValue: CGFloat = 24.0
```

### Funções/Tuplas

#### Utilizar retorno implícito
Exemplos:
##### Não preferível
```swift
func getName() -> String {
    return user.name
}
```

##### Correto
```swift
func getName() -> String {
    user.name
}
```

#### O código deve conter um whitespace entre o `)` e `{` 
Exemplos:
##### Não preferível
```swift
func someFunc(){
}
```

##### Correto
```swift
func someFunc() {
}
```

#### Fazer a abertura de `{` na mesma linha da declaração da função
Exemplos:
##### Não preferível
```swift
func someFunc()
{
}
```

##### Correto
```swift
func someFunc() {
}
```

#### Se precisa de mais que 3 parâmetros em uma função, talvez você esteja precisando de uma `struct`.
Exemplos:
##### Não preferível
```swift
func someFunc(param1: String, param2: String, param3: String, param4: String) { }
```

##### Correto
`SomeStruct.swift`
```swift
struct SomeStruct {
    let param1: String
    let param2: String
    let param3: String
    let param4: String
}
```
```swift
func someFunc(someStruct: SomeStruct) { }
```

#### O mesmo se aplica para tuplas, se precisar retornar mais que 2 itens, provavelmente você precisa de uma `struct` ou de um `array`.
Exemplos:
##### Não preferível
```swift
let student = ("Name", "Last name", 18, "name@email.com")
```

##### Correto
```swift
struct Student {
    let name: String
    let lastName: String
    let age: String
    let email: String
}
```

##### Não preferível
```swift
let students = ("Peter", "Mary", "John", "Michael")
```

##### Correto
```swift
let students = ["Peter", "Mary", "John", "Michael"]
```

### Inicializadores

#### Quando uma `struct` ou `class` necessitar de mais que 2 parâmetros, é preferível quebrar a linhas após a vírgula (`,`).
Exemplos:
##### Não preferível
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
    let email: String

    init(name: String, lastName: String, birthDate: String, email: String) {
        self.name = name
        self.lastName = lastName
        self.birthDate = birthDate
        self.email = email
    }
}
```

##### Correto
```swift
struct UserModel {
    let name: String
    let lastName: String
    let birthDate: String
    let email: String

    init(name: String, 
        lastName: String,
        birthDate: String, 
        email: String) {
        self.name = name
        self.lastName = lastName
        self.birthDate = birthDate
        self.email = email
    }
}
```

### Ordenação das declarações

#### Evitar declarar variáveis e métodos ao longo de qualquer parte do arquivo
Exemplos:
##### Não preferível
```swift
let view = UIView()

init() { }

@objc func didTapMainButton() { }

let mainButton = UIButton()
```

##### Correto
```swift
let view = UIView()
let mainButton = UIButton()

init() { }

@objc func didTapMainButton() { }
```

#### Construir classes e estruturas com a seguinte ordem:
- Protocols
- Enums (`public`, `internal`, `private`)
- Structs (`public`, `internal`, `private`)
- Views
- Constantes (`public`, `internal`, `private`)
- Variáveis (`public`, `internal`, `private`)
- Delegates (`public`, `internal`, `private`)
- Inits
- Lifecycle
- Funções públicas
- Funções internal
- Funções privadas
- Actions
- Extensions

Exemplo:
```swift
protocol ComponentDelegate: AnyObject {
    func didTapMainButton()
}

final class Component: UIViewController {
    enum Type {
        case left
        case right
    }

    let headerView = UIView()

    let padding = 16.0

    weak var delegate: ComponentDelegate?

    init() { }

    func viewDidLoad() {
        super.viewDidLoad()
    }
    
    func setupData() { }

    private func fetchUser() { }

    @objc private func didTapMainButton() { }
}

extension Component { }
```

#### Ordenar os modificadores na seguinte ordem
- `override`
- Nível de acesso (`public`, `open`, `private`)
- `final`
- `@objc`

Exemplos:
```swift
public final class Component: UIView {
    override public func layoutSubviews() { }

    @objc private func didTapMainButton() { }
}
```

### Lazy
Não utilizar `lazy` se não for realmente necessário, apesar de fazer com que aquele elemento só exista após a sua chamada, a [Apple](https://docs.swift.org/swift-book/LanguageGuide/Properties.html) recomenda utilizar apenas em casos que haja necessidade de maior processamento e casos de cálculos complexos. Em casos de necessidade de utilizar `self` na closure, prefira utilizar métodos auxiliares para isto.

Exemplos:
##### Não preferível
```swift
private lazy var mainButton: UIButton = {
    let button = UIButton()
    button.isEnabled = false
	button.addTarget(self, action: #selector(didTapMainButton), for: .touchUpInside)
    return button
}()
```

##### Correto
```swift
private let mainButton: UIButton = {
    let button = UIButton()
    button.isEnabled = false
    return button
}()

private func setupActions() {
    mainButton.addTarget(self, action: #selector(didTapMainButton), for: .touchUpInside)
}
```

### Self
#### Não utilizar `self` quando não houver necessidade
Exemplos:
##### Não preferível
```swift
func changeTitle(title: String) {
    self.titleLabel.text = title
}
```

##### Correto
```swift
func changeTitle(title: String) {
    titleLabel.text = title
}
```

### Delegates/Closures
#### Questionar se os argumentos passados por parâmetro de um `delegate` fazem sentido. Muitas vezes é comum colocar `self` como primeiro argumento do `delegate`
Exemplos:
##### Não preferível
```swift
protocol LoginViewControllerDelegate: AnyObject {
	func didTapLoginButton(_ viewController: UIViewController, button: UIButton)
}
```

##### Correto
```swift
protocol LoginViewControllerDelegate: AnyObject {  
	func didTapLoginButton()
}
```

#### Utilizar `weak self` quando uma closure utilizar algum atributo de `self`
Exemplos:
##### Não preferível
```swift
closure.action { isEnabled in
    self.mainButton.isEnabled = isEnabled
}
```

##### Correto
```swift
closure.action { [weak self] isEnabled in
    self?.mainButton.isEnabled = isEnabled
}
```

### Linhas em brancos

#### Não deve existir linha em branco entre a declaração de uma `class`/`struct` e seus atributos
Exemplos:
##### Não preferível
```swift
class LoginViewController {

    let headerView = UIView()

    init() { }
}
```

##### Correto
```swift
class LoginViewController {
    let headerView = UIView()

    init() { }
}
```

#### Não deve existir linha em branco após o último método
Exemplos:
##### Não preferível
```swift
class LoginViewController {
    ...

    func didTapMainButton() { }

}
```

##### Correto
```swift
class LoginViewController {
    ...

    func didTapMainButton() { }
}
```

#### Não deixar mais que uma linha em branco entre variáveis, constantes, métodos etc
Exemplos:
##### Não preferível
```swift
class LoginViewController {
    let view = UIView()


    init() { }
}
```

##### Correto
```swift
class LoginViewController {
    let view = UIView()

    init() { }
}
```

#### Não deixar linhas em branco entre variáveis/constantes/views
Exemplos:
##### Não preferível
```swift
class LoginViewController {
    let view = UIView()

    let button = UIButton()

    let title = Strings.title
}
```

##### Correto
```swift
class LoginViewController {
    let view = UIView()
    let button = UIButton()

    let title = Strings.title
}
```

#### Manter uma linha em branco entre declarações de variáveis e métodos e entre os métodos
Exemplos:
##### Não preferível
```swift
class LoginViewController {
    let button = UIButton()
    init() { }
    func didTapMainButton() { }
}
```

##### Correto
```swift
class LoginViewController {
    let button = UIButton()

    init() { }

    func didTapMainButton() { }
}
```

### Strings
Todos os textos do app devem estar no arquivo `Localized.strings` e disponível em todas as linguagens suportadas pelo aplicativo. Para acessar esses textos do arquivo `Localized.strings`, deve-se utilizar o arquivo `Strings.swift`.

Exemplos:
`Strings.swift`
```swift
struct Strings {
    static let cancel = "cancel".localized
    static let confirm = "confirm".localized
}
```

Utilização da classe `Strings.swift`:
```swift
let message = Strings.cancel
```

##### Não preferível
```swift
button.setTitle("Cancelar", for: .normal)
```
```swift
button.setTitle("Cancelar".localized, for: .normal)
```

##### Correto
```swift
button.setTitle(Strings.cancel, for: .normal)
```

### Strings multilinhas
#### Na utilização de strings multilinhas quebrar a linha após `"""`
Exemplos:
##### Não preferível
```swift
let message = """Lorem ipsum dolor sit amet, consectetur adipiscing elit, 
sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. 
Ut enim ad minim veniam, quis nostrud exercitation ullamco
"""
```

##### Correto
```swift
let message = """
Lorem ipsum dolor sit amet, consectetur adipiscing elit, 
sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. 
Ut enim ad minim veniam, quis nostrud exercitation ullamco
"""
```

### Opcionais
Não utilizar force unwrap em nenhuma ocasião ao longo do código, por mais garantia que se tenha que aquela propriedade exista, qualquer alteração pode facilmente ocasionar uma quebra no app.

Exemplos:
##### Não preferível
```swift
let cell = tableView.dequeueReusableCell(withIdentifier: identifier, for: indexPath) as! CustomCell
```

##### Correto
```swift
guard let cell = tableView.dequeueReusableCell(withIdentifier: identifier, for: indexPath) as? CustomCell else { 
    return 
}
```

##### Não preferível
```swift
func doSomething(param: String?) {
    receivedParam = param!
}
```

##### Correto
```swift
func doSomething(param: String?) {
    if let param = param {
        receivedParam = param
    }
}
```

O mesmo pensamento vale para o `unowned`, pois implicitamente ele faz um unwrap da propriedade em questão.

Exemplos:
##### Não preferível
```swift
unowned var delegate: Delegate
```

##### Correto
```swift
weak var delegate: Delegate?
```

### Views
As views devem ser construídas exclusivamente com `UIKit`, utilizando `View Code`, não utilizar `storyboard` ou `xib` para construção de telas para evitar conflitos, deixar mais leve a abertura dos arquivos, maior entendimento do que está acontecendo no código e facilidade no code review.

#### Criar as views utilizando closures quando houver necessidade de modificações do componente
Exemplos:
##### Não preferível
```swift
private let titleLabel = UILabel()

init() {
    titleLabel.numberOfLines = 0
    titleLabel.translatesAutoresizingMaskIntoConstraints = false
}
```

##### Correto
```swift
private let titleLabel: UILabel = {
    let label = UILabel()
    label.numberOfLines = 0
    label.translatesAutoresizingMaskIntoConstraints = false
    return label
}()
```

#### Evitar closures para criação de views quando não houver necessidade de modificações no componente 
Exemplos:
##### Não preferível
```swift
private let titleLabel: UILabel = {
    let label = UILabel()
    return label
}()
```

##### Correto
```swift
private let titleLabel = UILabel()
```

### Final
Recomendado utilizar `final` para as classes, inclusive de testes, para evitar heranças desnecessárias.

Exemplos:
##### Não preferível
```swift
class LoginView: UIView { }
```

##### Correto
```swift
final class LoginView: UIView { }
```

### Imports
Todos os `imports` devem ir no início do arquivo sem linhas em branco entre cada `import`. 

#### Não importar módulos redundantes
Exemplos:
##### Não preferível
```swift
import UIKit
import Foundation

class LoginViewController: UIViewController { }
```

##### Correto
```swift
import UIKit

class LoginViewController: UIViewController { }
```

#### Não importar módulos que não estejam sendo utilizados no arquivo
Exemplos:
##### Não preferível
```swift
import Foundation

struct User {
	let name: String
	let lastName: String
}
```

##### Correto
```swift
struct User {
	let name: String
	let lastName: String
}
```

#### Todos os arquivos do tipo `Swift` devem terminar com a extensão `.swift`

### Comentários
Comentários no código são desencorajados, pois um código bem organizado e padronizado é o suficiente para servir de documentação.

### MARKs
Utilizar os `MARKs` a seguir para facilitar a leitura do código:
- `// MARK: - Views`
- `// MARK: - Properties`
- `// MARK: - Initializer`
- `// MARK: - Life Cycle`
- `// MARK: - Other Methods`
- `// MARK: - Actions`
- `// MARK: - ExtensionName`
- `// MARK: - ProtocolName`

### Copyright Statement
Não utilizar o `Copyright Statement` no cabeçalho do código, toda informação sobre histórico de modificação e criação pode ser verificada através do git.

### Código não utilizado
Códigos que não serão utilizados no momento de sua criação não devem entrar para a branch principal, assim como, código que não serão mais utilizados devem ser excluídos do repositório.

### Frameworks
Não adicionar ao projeto frameworks e bibliotecas sem um motivo claro e sem discutir com o time. Frameworks e bibliotecas resolvem um problema relacionado ao tempo de desenvolvimento, porém adiciona uma complexidade excessiva ao código e pode gerar acoplamentos com mais facilidade. Optar sempre por fazer as coisas nativamente e de forma simples.
