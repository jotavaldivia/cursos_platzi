##RENDER Y COMPOSITION EN REACT

Los principios de diseño en React:

Abstracciones comunes: se refiere que a React no quiere incluir código inútil en su core, código que sea demasiado especifico para caso de uso demasiado concreto. Sin embargo, existen excepciones.

Interoperabilidad: React trata de trabajar bien con otras bibliotecas de interfaz de usuario.

Estabilidad: React va a mantener sus apis, componentes, funcionamiento, etc… aunque estén descontinuados para no romper el código que usamos.

Válvulas de escape: Cuando React quiera descontinuar un patrón que no les gusta, es sus responsabilidad considerar todos los casos de uso existentes para él, y antes de descontinuarlo educar a la comunidad respecto a las alternativas.

Experiencia de desarrollo: el objetivo de React no es solo que con su código podamos solucionar nuestro problemas también van a buscar una solución que nos den una buena experiencia y disfrute.

Implementación: Siempre que sea posible React preferirá código aburrido a código elegante. El código es descartable y cambia a menudo. Así que es importante que no introduzca nuevas abstracciones internas al menos que sea absolutamente necesario. Código detallado que sea fácil de mover, cambiar y eliminar es preferido sobre código elegante que esté abstraído de manera prematura y que sea difícil de cambiar.

Optimizado para instrumentación: React siempre va a buscar el nombre mas distintivos y detallados(no necesariamente nombres largos).

Dogfooding: significa que React va a periodizar la implementación de funcionalidades que necesite su empresa, Facebook, Esto tiene la ventaja no solo para su empresa sino también a todos los desarrolladores que utiliza React.

Planificación: Acá es donde nosotros dividimos nuestras responsabilidades de los que debemos hacer y lo que tiene que hacer React por detrás con las descripciones que le hacemos.

Configuración: React cree que una configuración global no funciona bien con la composición. Dado que la composición es central en React, no proveen configuración global en el código. React siempre se asegurara que nosotros tengamos compatibilidad entre cualquier librería y aplicación que utilicemos React.

Depuración: se refiere que a React siempre va a dejarte pistas un rastro predecible, donde podamos buscar los errores en nuestra aplicación.

Composición: Next Class

**Componentes y colocación del estado**

La Composición de componentes indica que cada componente debe darnos mucha libertad para elegir donde y como usarlo
Cada componente debe realizar una tarea en específica, pero no debe de decirnos como usar esa solución que nos brinda, debe de ser flexible al momento de utilizarlo
Nos permitirá
Tener componentes mucho más fáciles de integrar al resto de componentes
Nos facilitará reutilizar o hacer cambios en nuestros componentes
Ejemplo

Una solución para renderizar una tarea podría ser este componente

```
const App = () => (
	<TodoList todos={todos} />
);
const TodoList = ({todos}) => (
	<section>
		{todos.map(todo => (
			<TodoItem {...todo} />
		))}
	</section>
)

```

Existe otra forma de realizar esta tarea, y es que el componente App tenga también la tarea de renderizar cada tarea, esta manera nos dará mayor flexibilidad con el componente de TodoList

```
const TodoList = ({children}) => (
	<section>
		{children}
	</section>
)
const App = () => (
	<TodoList>
		{todos.map(todo => (
			<TodoItem {...todo} />
		))}
	</TodoList>
);
```

Colocación del estado
¿Dónde va tu estado?

Máxima cercanía a la relevancia
El estado ira según al área donde se aplique el mismo
Stateful vs. stateless
Se refiere a no tener revuelo entre componentes que manejan lógica y estado con los componentes que solo renderizan elementos
Se puede usar ambos principios siempre si los entendemos muy bien

Pensar en lo más grande y poco a poco ir a lo más especifico

¿Necesitas React Context?
Al usar composición de componentes puede ser una alternativa interesante en caso de que el árbol de componentes no sea demasiado profundo.

Ventajas

No será necesario pasar props por cada componente intermedio entre donde se encuentre el dato inicialmente hasta su destino
Podrás entender la aplicación con entender lo que está pasando en un archivo
Ejemplo

```
const App = () => {
	const [state, setState] = useState();
	return (
		<>
			<TodoHeader>
				<TodoCounter />
				<TodoSearch onSearch={setState} />
			</TodoHeader>
			<TodoList>
				{state.todos.map(todo => <TodoItem {...todo} {...state} />)}
			</TodoList>
		</>
	);
}

```

Puedes observar que el componente se puede comunicar sin mucha complicación directamente con sus componentes hijos, nietos o incluso bisnietos.

De esta manera se está reduciendo la complejidad de los componentes intermedios
Además que se puede observar legibilidad con solo ver un archivo
Pero esta manera es insostenible en el caso que el árbol de componentes sea muy grande, pero aún se puede utilizar composición de componentes al utilizar React context

Cuando los componentes nietos de App no solo son nietos, sino también componentes hijos, podemos pasarles props directamente y mejorar su comunicación.

–

Casi siempre que llamamos a un componente… pos lo llamamos y ya. 😅

```
function App() {
  return (
    <TodoHeader />
  );
}

function TodoHeader() {
  return (
    <TodoCounter />
  );
}
```

Esto implica que para compartir el estado debemos pasar props y props y props por cada componente intermedio entre App y los componentes que realmente necesiten esas props en cualquier lugar de la jerarquía. 😓

```
function App() {
  const [state, setState] = React.setState(initialState);

  return (
    <TodoHeader state={state} setState={setState} />
  );
}

function TodoHeader({ state, setState }) {
  return (
    <header>
      <TodoCounter state={state} setState={setState} />
    </header>
  );
}
```

Pero otra forma de trabajar es que App no solo llame a sus componentes directamente hijos, sino que también llamen a los siguientes componentes en la jerarquía de la aplicación. 😮

```
function App() {
  return (
    <TodoHeader>
      <TodoCounter />
    </TodoHeader>
  );
}

function TodoHeader({ children }) {
  return (
    <header>
      {children}
    </header>
  );
}
```

Y esta nueva forma de trabajar implica que ya no tenemos que pasar props y props y props entre App y el resto de componentes para compartir el estado, sino que App puede comunicarse directamente con el componente que realmente necesita ese estado. 🤩

```
function App() {
  const [state, setState] = React.setState(initialState);

  return (
    <TodoHeader>
      <TodoCounter state={state} setState={setState} />
    </TodoHeader>
  );
}
```
