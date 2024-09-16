# Prática da Semana - 16/09 à 20/09 | Tema - Lidando com eventos no React

**O tratamento de eventos é fundamental para entender como o React processa eventos do navegador e atualiza o DOM. Essa prática aborda como configurar manipuladores de eventos e prossegue para técnicas mais avançadas de gerenciamento de eventos. Você aprenderá como criar eventos em componentes React, passar argumentos para os manipuladores e evitar comportamentos padrão.**

## **Criando o projeto**

Abra a pasta desse repositório prática que você clonou com o VSCode, abra o terminal e inicie um projeto react:

```prompt
npm create vite@latest
```

Após nomear o projeto, entrar na pasta e instalar as dependências com:

```prompt
cd nome_do_projeto
npm install
```

Podemos limpar os arquivos padrões que vem com o React + vite e rodar a aplicação para testar:

```prompt
npm run dev
```

## **Como criar manipuladores de eventos em componentes React**

### **Evento onClick**
A criação de um evento no React começa com a anexação do nome do evento ao elemento que o disparará, com a função do manipulador referenciada nele. 

**Exemplo:**
```html
<button onClick={handleClick}>
  Clique aqui
</button>
```

Seguido da definição de uma função manipuladora, ou seja, a função que será executada quando o evento for disparado.

**Exemplo:**
```javascript
const handleClick = () => {
   alert('You clicked me');
};
```

Para isso, dentro da pasta `src` crie a pasta `Components`. Dentro dessa pasta iremos criar um componente chamado `Button.jsx` com o seguinte código: 

```javascript
import React from 'react'

const Button = () => {
    const handleClick = () => {
        alert('Você clicou no botão!');
    }

    return (
        <>
            <button onClick={handleClick}>
                Clique aqui
            </button>
        </>
    )
}

export default Button
```

Agora que temos nosso componente botão, vamos importá-lo e usá-lo no `App.jsx` de nossa aplicação:

```javascript
import './App.css'
import Button from './Components/Button'

function App() {
  return (
    <>
      <Button />
    </>
  )
}

export default App

```

## **Atualizando um estado com base em um evento**

Para atualizar um estado no react, utilizamos do hook `useState`. Vamos criar agora um novo componente em nossa aplicação para reforçar esse conceito, crie na pasta `Components` o arquivo `CounterButton.jsx`:

```javascript
import React from 'react'
import { useState } from 'react'

const CounterButton = () => {
    const [count, setCount] = useState(0);

    return (
        <div>
            <button onClick={() => setCount(count + 1)}>Incrementar ⬆️</button>
            <h1>{count}</h1>
            <button onClick={() => setCount(count - 1)}>Decrementar ⬇️</button>
        </div>
    )
}

export default CounterButton
```

Renderize isso em tela:

```javascript
import './App.css'
import Button from './Components/Button'
import CounterButton from './Components/CounterButton'

function App() {
  return (
    <>
      {/* <Button /> */}
      <CounterButton />
    </>
  )
}

export default App
```

### **Evento onChange**

`onChange` e `onSubmit` são outros eventos populares no React. `onChange` é usado em elementos de entrada de texto(`input`) e `onSubmit` são usados em elementos `form`.

Dentro da pasta `Components` crie um novo componente chamado input, para utilizarmos esses eventos na prática:

```javascript
import React, { useState } from 'react'

const Input = () => {
    const [inputValue, setInputValue] = useState('');

    const handleChange = () => {
        setInputValue(event.target.value);
    }

    return (
        <div>
            <input 
                type="text"
                value={inputValue}
                onChange={handleChange}
                placeholder='Digite algo...'
            />
            <p>Você digitou: {inputValue}</p>
        </div>
    )
}

export default Input
```

Renderize isso em tela:

```javascript
import './App.css'
import Button from './Components/Button'
import CounterButton from './Components/CounterButton'

function App() {
  return (
    <>
      {/* <Button /> */}
      <CounterButton />
    </>
  )
}

export default App
```

Veremos um exemplo de `onSubmit` na seção sobre como evitar comportamentos padrãa do navegador ao enviar um `form`

Outros exemplos de eventos incluem eventos de teclado como `onKeyDown`, `onKeyPress`, e `onKeyUp`, eventos de mouse como `onMouseUp`, `onMouseDown`, `onMouseEnter`, `onDrag`, e mais. Qualquer evento popular em JavaScript está disponível em React. A única diferença é que os eventos são escritos em camelCaseReact.

## **Passando argumentos/parâmetros para funções manipuladoras de eventos**

Passar argumentos para manipuladores de eventos no React é um requisito comum quando você precisa executar ações em dados específicos associados a um evento. Por exemplo, excluir ou editar um recurso.

Para fazer isso, a função manipuladora precisa receber um parâmetro:

**Exemplo:**
```javascript
const handleClick = (item) => {
   console.log('Button click for:', item);
 };
```

Em seguida, você passa um argumento correspondente a esse parâmetro para a função anônima do evento:

```html
<button onClick={() => handleClick(item)}>Click Me</button>
```

Vamos agora criar um componente novo, para vermos isso na prática. Dentro da pasta `Components` crie o componente `TaskManager.jsx`:

```javascript
import React, { useState } from 'react'

const TaskManager = () => {
    // Tarefas predefinidas
    const [tasks, setTasks] = useState([
        { id: 1, text: 'Ler um artigo' },
        { id: 2, text: 'Ler um livro' },
        { id: 3, text: 'Estudar React' },
        { id: 4, text: 'Dormir ' },
    ])

    // Função para deletar uma tarefa, de acordo com o ID da mesma
    const deleteTask = (taskId) => {
        console.log(taskId);

        setTasks((currentTasks) => {
            return currentTasks.filter((task) => task.id !== taskId)
        });
        alert('Deletada a task com ID: ' + taskId);
    }


    return (
        <div>
            <ul>
                {tasks.map((task) => (
                    <li key={task.id}>
                        {task.text}
                        <button onClick={() => deleteTask(task.id)}>🗑️</button>
                    </li>
                ))}
            </ul>
        </div>
    )
}

export default TaskManager
```

Renderize isso em tela:

```javascript
import './App.css'
import Button from './Components/Button'
import CounterButton from './Components/CounterButton'
import Input from './Components/Input'
import TaskManager from './Components/TaskManager'

function App() {
  return (
    <>
      {/* <Button /> */}
      {/* <CounterButton /> */}
      {/* <Input /> */}
      <TaskManager />
    </>
  )
}

export default App
```

## **Padrões comuns no tratamento de eventos React**

Os padrões de manipulação de eventos referem-se às técnicas para manipular interações do usuário dentro dos componentes React.

Dentro da pasta `Components`, crie o componente `CheckLogin.jsx`:

```javascript
import React, { useState } from 'react'

const CheckLogin = () => {
    const [isLoggedIn, setLoggedIn] = useState(false);

    const toggleLogin = () => {
        setLoggedIn(!isLoggedIn);
    }

    const handleLogin = () => {
        if(isLoggedIn) {
            alert('AVISO: Usuário está logado!');
        }
        else {
            alert('AVISO: Usuário não está logado!');
        }
    }
    

    return (
        <div>
            <h2>
                {isLoggedIn ? 'Usuário logado' : 'Usuário não logado'}
            </h2>
            <button onClick={handleLogin}>
                Checar estado
            </button>
            <button onClick={toggleLogin}>
                { isLoggedIn ? 'Log Out' : 'Log In'}
            </button>
        </div>
    )
}

export default CheckLogin
```

Renderize isso em tela:

```javascript
import './App.css'
import Button from './Components/Button'
import CheckLogin from './Components/CheckLogin'
import CounterButton from './Components/CounterButton'
import Input from './Components/Input'
import TaskManager from './Components/TaskManager'

function App() {
  return (
    <>
      {/* <Button /> */}
      {/* <CounterButton /> */}
      {/* <Input /> */}
      {/* <TaskManager /> */}
      <CheckLogin />
    </>
  )
}

export default App
```

## **Melhores práticas para tratamento eficiente de eventos no React (EXTRA)**

### Evite usar funções de seta anônimas dentro de eventos

Parece conveniente usar funções de seta diretamente em eventos, como `onClick={() => console.log('button clicked')})`. A desvantagem disso é que pode levar a problemas de desempenho porque uma nova função é criada em cada renderização.

Sempre defina a função do manipulador a ser executada quando o evento for disparado fora do método render para evitar esses problemas de desempenho.

### Memorize eventos com o gancho useCallback

Para componentes que são renderizados novamente com frequência, memorizar os manipuladores nele com o hook `useCallback` pode evitar renderizações novamente desnecessárias. Isso é útil ao passar eventos como props para componentes filhos que podem ser renderizados novamente desnecessariamente.

### Evite comportamento padrão quando necessário

Use `event.preventDefault()` em seus manipuladores de eventos quando precisar impedir que o navegador execute ações padrão, como enviar um formulário. No entanto, você deve usar esse método com prudência para evitar bloquear comportamentos do navegador desnecessariamente.

### Limpar ouvintes de eventos

Se você configurar seus ouvintes de eventos em `useEffect`, sempre retorne uma função de limpeza para remover o ouvinte de eventos. Caso contrário, isso causará vazamentos de memória.

### Manipuladores de eventos de teste

Certifique-se de que seus manipuladores de eventos estejam cobertos em seus testes unitários e de integração. Frameworks de teste como Jest combinados com React Testing Library podem ajudar a verificar se seus manipuladores de eventos estão funcionando conforme o esperado.

# Conclusão

Nesta prática, você aprendeu os fundamentos do tratamento de eventos no React, com foco em como usar o sistema de eventos sintéticos do React para criar eventos em aplicativos da web React.

Exploramos a definição de manipuladores de eventos, a passagem de argumentos e a prevenção de comportamentos padrão do navegador para melhorar a experiência do usuário. Com esse conhecimento básico, você pode seguir aprofundando em React e implementar o tratamento de eventos em seus projetos para melhorar tanto a funcionalidade quanto o engajamento do usuário.

Agradeço aos que chegaram até aqui, focos nos estudos!

Abraços, 
Fernando Zuchi

