# Optimización de Código



## 1. Código en JavaScript

### Código Original
```javascript
function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = "";
  for (let i = 0; i < items.length; i++) {
    let listItem = document.createElement("li");
    listItem.innerHTML = items[i];
    list.appendChild(listItem);
  }
}
```

Problemas Identificados
* Manipulación Excesiva del DOM: Cada llamada a appendChild provoca reflujo y repintado, lo que impacta negativamente en el rendimiento.
* Reinicialización Completa: list.innerHTML = "" limpia todo el contenido previo, lo cual puede ser redundante.
* Estructura de Bucle Ineficiente: La manipulación del DOM dentro de cada iteración introduce sobrecarga.

```javascript
function updateList(items) {
  const list = document.getElementById("itemList");
  const fragment = document.createDocumentFragment();
  
  list.innerHTML = ""; // Limpiar la lista
  items.forEach(item => {
    const listItem = document.createElement("li");
    listItem.innerHTML = item;
    fragment.appendChild(listItem);
  });

  list.appendChild(fragment); // Actualización en lote del DOM
}
```


## Cambios Realizados
* Batch Updates: Uso de DocumentFragment para minimizar las operaciones en el DOM.
* Iteración Moderna: Uso de forEach en lugar de un bucle for tradicional para mayor claridad.
* Optimización de la Limpieza: Se retiene la funcionalidad de reinicialización, pero se mejora la eficiencia en las actualizaciones.
## Resultados Esperados
* Reducción significativa de reflujo y repintado.
* Mayor rendimiento y código más limpio.

## 2. Código en Java

### Código Original
```java
public class ProductLoader {
    public List<Product> loadProducts() {
        List<Product> products = new ArrayList<>();
        for (int id = 1; id <= 100; id++) {
            products.add(database.getProductById(id));
        }
        return products;
    }
}
```

Problemas Identificados
* Consultas Redundantes: Cada iteración ejecuta una consulta individual a la base de datos, aumentando la latencia.
* Cuello de Botella: Las múltiples consultas incrementan el tiempo de carga y el uso de recursos.
* Falta de Optimización: No se utiliza un enfoque de consulta en bloque.

```java
public class ProductLoader {
    public List<Product> loadProducts() {
        return database.getProductsByIds(
            IntStream.rangeClosed(1, 100).boxed().collect(Collectors.toList())
        );
    }
}
```


## Cambios Realizados
* Consulta en Bloque: Se introduce un método getProductsByIds para realizar una única consulta.
* Uso de Streams: Se utiliza IntStream para generar el rango de IDs de manera declarativa.
* Reducción de Carga: Eliminación de múltiples viajes al servidor.
## Resultados Esperados
* Mejora en el rendimiento al reducir el tiempo de consulta.
* Código más escalable y fácil de mantener.

## 3. Código en C#

### Código Original
```csharp
public List<int> ProcessData(List<int> data) {
    List<int> result = new List<int>();
    foreach (var d in data) {
        if (d % 2 == 0) {
            result.Add(d * 2);
        } else {
            result.Add(d * 3);
        }
    }
    return result;
}
```

Problemas Identificados
*  Cálculos Redundantes: Ambas ramas del condicional realizan operaciones de multiplicación.
* Lógica Compleja: El uso de if-else introduce ramificaciones innecesarias.
* Estructura Ineficiente: La creación explícita de una lista agrega complejidad.

```csharp
public List<int> ProcessData(List<int> data) {
    return data.Select(d => d * (d % 2 == 0 ? 2 : 3)).ToList();
}
```


## Cambios Realizados
* Lógica Simplificada: Consolidación del cálculo en una única expresión.

* Mejoras en Mantenibilidad: Eliminación de la creación manual de listas y la ramificación explícita.
## Resultados Esperados
* Reducción del tiempo de procesamiento.
* Código más claro y eficiente.