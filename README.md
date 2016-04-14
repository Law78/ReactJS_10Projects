# ReactJS Build 10 Projects
Sezione 2 di ReactJS build 10 projects


Codice del corso Udemy.

Primi componenti in React.

React ragiona per componenti. Tutto in React è un componente. Questi componenti li creo tramite la createClass che vuole un oggetto a cui aggiungo, solitamente, le funzioni di render (obbligatoria sempre!) e le funzioni per impostare le proprietà di base del componente e i loro valori e tipi di default (getDefaultProps, propTypes).
[LIVE PROJECT](https://jsfiddle.net/wz9vfku5/)

Carico nello HEAD del file HTML i file js di React e React-dom ed inoltre il js di babel-core/browser.min.js
https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.23/browser.min.js
La versione 6 ha problemi...

Il primo componente che provo a creare è del tipo:
```
<div id="area"></div>
<script>
ReactDOM.render(
  <h1>Hello World</h1>,
  document.getElementById('area')
);
</script>
```
In pratica ho una funzione render a cui passo del codice HTML e una istruzione JS che passa l'elemento su cui quel codice HTML verrà posto.

Andiamo avanti e creo un nuovo componente:
```
var HelloWorld = React.createClass({})
```
devo passare un oggetto che mi permette di passare alla funzione render direttamente il mio componente:

```
ReactDOM.render(
  <HelloWorld />,
  document.getElementById('area')
);
```

come fosse un vero e proprio TAG di HTML. Nel nostro oggetto che vado a passare mi serve la funzione render

```
var HelloWorld = React.createClass({
  render: function(){
    return(
      <h1>Hello World</h1>
    );
  }
});
```

Devo avere un TAG ROOT che contiene gli altri TAG HTML all'intero, cioè li wrappa.
Adesso vogliamo avere delle proprietà associate al nostro elemento:
<HelloWorld name="Lorenzo" />
Passiamo il valore di nome al mio componente, che lo posso accedere tramite l'espressione: {this.props.name}:
(Ciò che scrivo tra le parentesi graffe è javascript!)

```
var HelloWorld = React.createClass({
  render: function(){
    return(
      <h1>Hello {this.props.name}</h1>
    );
  }
});
```

Quando React crea il nostro componente, abbiamo detto che l'unica funzione obbligatoria da passare è la render. Possiamo inserire un'altra funzione, che React crea per noi, ma che possiamo controllare a nostro piacimento che è la getDefaultProps, che ritorna un oggetto con le nostre proprietà inizializzate:

```
var HelloWorld = React.createClass({
  getDefaultProps: function(){
    return{
      name: "Lorenzo"
    }
  },
  
  render: function(){
    return(
      <h1>Hello {this.props.name}</h1>
    );
  }
});
```

E' buona pratica definire il tipo delle proprietà, anche per il self-documentation, utilizzando un oggetto chiamato propTypes (ho aggiunto anche il .isRequired se il dato è richiesto!) :

```
var HelloWorld = React.createClass({

  propTypes: {
    name: React.PropTypes.string.isRequired
  },
  
  getDefaultProps: function(){
    return{
      name: "Lorenzo"
    }
  },
  
  render: function(){
    return(
      <h1>Hello {this.props.name}</h1>
    );
  }
});
```

Se passassi un boolean otterrei un errore:
```
ReactDOM.render(
  <HelloWorld name={false}/>,
  document.getElementById('area')
);
```

Se ho isRequired e provo a togliere la getDefaultProps, ottengo un errore.

Posso usare dei conditionals:

```
var HelloWorld = React.createClass({

  propTypes: {
    name: React.PropTypes.string,
    isPerson: React.PropTypes.bool
  },
  
  getDefaultProps: function(){
    return{
      name: "Lorenzo"
    }
  },
  
  render: function(){
    var greeting = "World";
    
    if(this.props.isPerson){
      greeting = (<Person name={this.props.name}/>)
    }
    return(
      <h1>Hello {greeting}</h1>
    );
  }
});

var Person = React.createClass({
  propTypes:{
    name: React.PropTypes.string
  },
  
  getDefaultProps: function(){
    return{
      name: "Lorenzo"
    };
  },
  
  render: function(){
    return(
      <span>{this.props.name}</span>
    );
  }
});

ReactDOM.render(
  <HelloWorld name="Giuseppe" isPerson={true}/>,
  document.getElementById('area')
);
```
Un altro importante metodo da vedere all'interno dell'oggetto createClasse({}) è il getInitialState, ovvero una funzione che ritorna un oggetto con i valori che vogliamo impostare inizialmente e la componentDidMount, ovvero per cambiare lo stato quando il componente è pronto per il rendering:
```
...
getDefaultProps: function(){
  return{
    name: "Lorenzo"
  }
},
  
getInitialState: function(){
  return{
    value: 1
  }
}

render: function(){
  var greeting = "World";
    
  if(this.props.isPerson){
    greeting = (<Person name={this.props.name}/>)
  }
  return(
    <div>
      <h1>Hello {greeting}</h1>
      {this.state.value}
    <div>
  );
}
  ...
```
