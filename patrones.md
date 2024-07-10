# Patrón Factory Method
El propósito del patrón Factory Method es definir una interfaz para crear un objeto, pero permitir a las subclases decidir cuál clase instanciar. Esto permite a una clase delegar la responsabilidad de creación de objetos a sus subclases, proporcionando una forma de encapsular la creación de objetos y permitir que las subclases alteren el tipo de objetos que se crean.

# Cuando se utiliza
Cuando una clase no puede anticipar la clase de objetos que debe crear: Si una clase no sabe qué tipo de objetos debe crear, puede delegar la responsabilidad a subclases específicas.
Cuando una clase quiere que sus subclases especifiquen los objetos que se deben crear: Esto es útil en situaciones donde la clase base necesita utilizar los objetos, pero las subclases concretas deben decidir qué tipo de objeto se necesita.
Para desacoplar el código que utiliza los objetos del código que crea los objetos: Esto promueve la reutilización de código y facilita el mantenimiento y la extensión de las aplicaciones. 

``` java
// Producto abstracto
public abstract class Message {
    public abstract void send();
}

// Productos concretos
public class TextMessage extends Message {
    @Override
    public void send() {
        System.out.println("Sending a text message");
    }
}

public class EmailMessage extends Message {
    @Override
    public void send() {
        System.out.println("Sending an email message");
    }
}

// Creador abstracto
public abstract class MessageCreator {
    public void sendMessage() {
        Message msg = createMessage();
        msg.send();
    }

    // Método Factory que las subclases implementarán
    protected abstract Message createMessage();
}

// Creadores concretos
public class TextMessageCreator extends MessageCreator {
    @Override
    protected Message createMessage() {
        return new TextMessage();
    }
}

public class EmailMessageCreator extends MessageCreator {
    @Override
    protected Message createMessage() {
        return new EmailMessage();
    }
}

// Uso del patrón Factory Method
public class Main {
    public static void main(String[] args) {
        MessageCreator creator;

        // Crear y enviar un mensaje de texto
        creator = new TextMessageCreator();
        creator.sendMessage(); // Imprime "Sending a text message"

        // Crear y enviar un mensaje de correo electrónico
        creator = new EmailMessageCreator();
        creator.sendMessage(); // Imprime "Sending an email message"
    }
}
```


# Patrón Decorator
El propósito del patrón Decorator es agregar responsabilidades adicionales a un objeto de manera dinámica. El patrón Decorator proporciona una alternativa flexible a la herencia para extender la funcionalidad de las clases.


# Cuando se utiliza

## Para añadir responsabilidades a objetos individuales de manera dinámica y transparente:
Si necesitas agregar funcionalidades adicionales a un objeto específico en tiempo de ejecución sin afectar a otros objetos de la misma clase.

## Para extender la funcionalidad de una clase sin utilizar herencia:
Cuando no puedes o no deseas usar herencia debido a la necesidad de combinar múltiples comportamientos de manera flexible.

## Para evitar la proliferación de subclases:
Cuando agregar comportamientos mediante subclases resultaría en una gran cantidad de subclases para cubrir todas las combinaciones posibles de comportamientos.

``` java
// Componente
interface Coffee {
    String getDescription();
    double getCost();
}

// Componente concreto
class SimpleCoffee implements Coffee {
    @Override
    public String getDescription() {
        return "Simple coffee";
    }

    @Override
    public double getCost() {
        return 5.0;
    }
}

// Decorador concreto
class MilkDecorator implements Coffee {
    private Coffee coffee;

    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getDescription() {
        return coffee.getDescription() + ", milk";
    }

    @Override
    public double getCost() {
        return coffee.getCost() + 1.5;
    }
}

// Uso del patrón Decorator
public class Main {
    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        System.out.println(coffee.getDescription() + " $" + coffee.getCost());

        coffee = new MilkDecorator(coffee);
        System.out.println(coffee.getDescription() + " $" + coffee.getCost());
    }
}
```

# Patrón Observer
El propósito del patrón Observer es establecer una relación de dependencia uno-a-muchos entre objetos, de manera que cuando un objeto cambia su estado, todos sus dependientes son notificados y actualizados automáticamente. Esto promueve una comunicación efectiva entre los objetos y facilita la implementación de sistemas reactivos.

# Cuando se utiliza
 ## Sistemas de Eventos: 
Cuando una acción en un objeto debe desencadenar reacciones en otros objetos, como en interfaces gráficas de usuario (GUI) donde los eventos de usuario deben ser manejados por múltiples componentes.

## Notificaciones: 
En aplicaciones móviles o web, para implementar sistemas de notificación donde los cambios en el servidor deben ser reflejados en la interfaz de usuario.

## Modelos Reativos: 
En aplicaciones donde el estado del modelo debe reflejarse automáticamente en la vista sin necesidad de que la vista consulte constantemente el modelo.

``` java
import java.util.ArrayList;
import java.util.List;

// Interfaz Observer
interface Observer {
    void update(String message);
}

// Interfaz Subject
interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}

// Implementación concreta de Subject
class ChatRoom implements Subject {
    private List<Observer> users = new ArrayList<>();
    private String message;

    public void newMessage(String message) {
        this.message = message;
        notifyObservers();
    }

    @Override
    public void registerObserver(Observer o) {
        users.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        users.remove(o);
    }

    @Override
    public void notifyObservers() {
        for (Observer user : users) {
            user.update(message);
        }
    }
}

// Implementación concreta de Observer
class User implements Observer {
    private String name;

    public User(String name) {
        this.name = name;
    }

    @Override
    public void update(String message) {
        System.out.println(name + " received message: " + message);
    }
}

// Uso del patrón Observer
public class Main {
    public static void main(String[] args) {
        ChatRoom chatRoom = new ChatRoom();
        
        Observer user1 = new User("Alice");
        Observer user2 = new User("Bob");
        
        chatRoom.registerObserver(user1);
        chatRoom.registerObserver(user2);
        
        chatRoom.newMessage("Hello, everyone!");
        
        chatRoom.removeObserver(user1);
        
        chatRoom.newMessage("Goodbye, Alice!");
    }
}
```

# 2.Analisis del caso 
 ## Factory Method

Propósito: Proporcionar una interfaz para crear objetos en una superclase, pero permite a las subclases alterar el tipo de objetos que se crearán.

### Aplicación en la gestión de tareas:

Creación de Tareas: Utilizar el Factory Method para crear diferentes tipos de tareas (simples, recurrentes, con alta prioridad). Cada tipo de tarea tendrá sus propias propiedades y métodos específicos, permitiendo una creación controlada y coherente de objetos de tareas según las necesidades del usuario.

## Singleton

Propósito: Asegurar que una clase tenga solo una instancia y proporcionar un punto de acceso global a ella.

### Aplicación en la gestión de tareas:

Configuración de la Aplicación: La configuración global de la aplicación, como las preferencias del usuario, o la gestión de la conexión a la base de datos, se puede gestionar mediante un Singleton para garantizar que solo exista una instancia en toda la aplicación.

## Adapter

Propósito: Permitir que interfaces incompatibles trabajen juntas convirtiendo la interfaz de una clase en otra interfaz que el cliente espera.

### Aplicación en la gestión de tareas:

Integración con Servicios Externos: Si la aplicación necesita integrarse con un servicio externo para la generación de reportes o para la sincronización con otras herramientas de gestión, el patrón Adapter puede convertir la interfaz del servicio externo a la interfaz esperada por la aplicación, facilitando la integración sin modificar el código existente.


## Composite

Propósito: Permitir a los clientes tratar de manera uniforme objetos individuales y composiciones de objetos.

### Aplicación en la gestión de tareas:

Gestión de Subtareas: Las tareas pueden contener múltiples subtareas. Utilizando el patrón Composite, tanto las tareas como las subtareas pueden ser tratadas de manera uniforme, permitiendo que las operaciones (como agregar, eliminar, o mostrar) se apliquen de manera recursiva en la jerarquía de tareas.
 
## Observer

Propósito: Definir una dependencia uno-a-muchos entre objetos, de manera que cuando uno de los objetos cambie su estado, todos sus dependientes sean notificados y actualizados automáticamente.

### Aplicación en la gestión de tareas:

Notificación de Cambios: Cuando una tarea es asignada a un usuario o su estado cambia, todos los usuarios observadores interesados pueden ser notificados automáticamente, facilitando la comunicación en tiempo real y manteniendo a todos los usuarios informados sobre el progreso y los cambios.

## Command

Propósito: Encapsular una solicitud como un objeto, permitiendo parametrizar a los clientes con diferentes solicitudes, encolar o registrar solicitudes, y soportar operaciones que pueden deshacerse.

### Aplicación en la gestión de tareas:

Operaciones de Tareas: Las operaciones de creación, edición y eliminación de tareas pueden ser implementadas como comandos. Esto permite implementar fácilmente las funcionalidades de deshacer y rehacer, proporcionando a los usuarios la flexibilidad de revertir cambios y manejar las operaciones de manera más robusta.