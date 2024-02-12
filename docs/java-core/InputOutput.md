# Input Output in Java

## java.io

**java.io** - это пакет в Java, предоставляющий классы для работы с потоками ввода/вывода (I/O).

### 1. System.in и System.out:

**System.in:** Представляет стандартный поток ввода, который, как правило, связан с консолью. Вы можете использовать System.in для чтения данных с клавиатуры.

**System.out:** Представляет стандартный поток вывода, который обычно связан с консолью. Вы можете использовать System.out для вывода данных на экран.

### 2. InputStreamReader и OutputStreamWriter:

**InputStreamReader:** Преобразует байтовые потоки ввода в символьные потоки ввода. Вы можете использовать его для чтения текстовых данных из потока ввода, например, из System.in.

**OutputStreamWriter:** Преобразует символьные потоки вывода в байтовые потоки вывода. Используется для записи текстовых данных в поток вывода, например, в System.out.

### 3. BufferedReader и BufferedWriter:

**BufferedReader:** Предоставляет буферизацию для символьного ввода. Это улучшает производительность, так как считывание происходит блоками, а не посимвольно. Может использоваться для чтения текстовых данных из Reader, такого как InputStreamReader.

**BufferedWriter:** Предоставляет буферизацию для символьного вывода. Позволяет записывать данные блоками вместо посимвольно. Может использоваться для записи текстовых данных в Writer, например, в OutputStreamWriter.

### 4. flush() и close():

**flush():** Принудительно выталкивает буфер, что означает запись всех накопленных данных в поток. Полезно, например, когда вы хотите убедиться, что все данные записаны в файл или отправлены через сетевое соединение.

**close():** Закрывает поток ввода/вывода. Закрытие потока важно для освобождения ресурсов и предотвращения утечек. При закрытии также может быть вызван flush().

```java
public static void main(String[] args) {
        // Укажите путь к файлу, который вы хотите прочитать
        String filePath = "path/to/your/textfile.txt";

        // Используем try-with-resources для автоматического закрытия ресурсов
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;

            // Читаем файл построчно
            while ((line = reader.readLine()) != null) {
                System.out.println(line); // Выводим каждую строку на экран
            }
        } catch (IOException e) {
            // Обработка исключений, если произошла ошибка при чтении файла
            e.printStackTrace();
        }
    }
    // BufferedReader используется для буферизованного чтения данных, что улучшает производительность.
    // FileReader используется для чтения данных из файла. Вы можете указать путь к файлу в переменной filePath.
    // try-with-resources используется для автоматического закрытия BufferedReader. Это гарантирует, что ресурсы будут освобождены даже в случае возникновения исключения.
```

## java.nio

### 1. Channel (Канал): 

**Channel** представляет собой двусторонний поток данных, который может быть открыт для чтения, записи или обоих. 
Каналы могут быть файлами, сокетами, или другими источниками данных. 
Например, FileChannel используется для работы с файлами, а SocketChannel - для работы с сокетами.

```java
public static void main(String[] args) {
        Path filePath = Paths.get("path/to/your/output.txt");

        try (FileChannel fileChannel = FileChannel.open(filePath, StandardOpenOption.WRITE, StandardOpenOption.CREATE)) {
            String data = "Hello, Java NIO!";
            ByteBuffer buffer = ByteBuffer.wrap(data.getBytes());

            // Запись данных в файл с использованием FileChannel
            fileChannel.write(buffer);

            System.out.println("Data written to file successfully.");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

### 2. Buffer (Буфер):

**Buffer** представляет собой место для временного хранения данных, которые передаются между каналами и приложением. 
Буферы предоставляют методы для добавления данных в буфер и извлечения данных из буфера. 
Классы буферов включают ByteBuffer, CharBuffer, IntBuffer и другие.

```java
public static void main(String[] args) {
        // Создаем буфер для хранения 10 байт
        ByteBuffer buffer = ByteBuffer.allocate(10);

        // Помещаем данные в буфер
        buffer.put((byte) 'H');
        buffer.put((byte) 'e');
        buffer.put((byte) 'l');
        buffer.put((byte) 'l');
        buffer.put((byte) 'o');

        // Переключаем буфер в режим чтения
        buffer.flip();

        // Читаем данные из буфера
        while (buffer.hasRemaining()) {
            System.out.print((char) buffer.get());
        }
    }
```

### 3. Selector (Селектор): 

**Selector** позволяет одному потоку мониторить несколько каналов на предмет событий ввода-вывода (например, готовность к чтению или записи). 
Это позволяет эффективно обрабатывать множество каналов в одном потоке.

С помощью селектора мы можем использовать один поток вместо нескольких для управления несколькими каналами. 
Переключение контекста между потоками дорого обходится операционной системе , и, кроме того, каждый поток занимает память.

```java
public static void main(String[] args) {
        try (ServerSocketChannel serverSocketChannel = ServerSocketChannel.open()) {
            serverSocketChannel.bind(new InetSocketAddress(8080));
            serverSocketChannel.configureBlocking(false); // Неблокирующий режим

            Selector selector = Selector.open();
            serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

            while (true) {
                int readyChannels = selector.select();

                if (readyChannels == 0) {
                    continue;
                }

                Set<SelectionKey> selectedKeys = selector.selectedKeys();
                Iterator<SelectionKey> keyIterator = selectedKeys.iterator();

                while (keyIterator.hasNext()) {
                    SelectionKey key = keyIterator.next();

                    if (key.isAcceptable()) {
                        // Принимаем новое соединение
                        ServerSocketChannel serverChannel = (ServerSocketChannel) key.channel();
                        SocketChannel socketChannel = serverChannel.accept();
                        socketChannel.configureBlocking(false);
                        socketChannel.register(selector, SelectionKey.OP_READ);
                    } else if (key.isReadable()) {
                        // Читаем данные из клиента
                        SocketChannel socketChannel = (SocketChannel) key.channel();
                        ByteBuffer buffer = ByteBuffer.allocate(1024);
                        int bytesRead = socketChannel.read(buffer);

                        if (bytesRead > 0) {
                            buffer.flip();
                            while (buffer.hasRemaining()) {
                                System.out.print((char) buffer.get());
                            }
                            System.out.println();
                        } else if (bytesRead == -1) {
                            // Клиент отключился
                            socketChannel.close();
                        }
                    }

                    keyIterator.remove(); // Удаляем обработанный ключ
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```


## Отличия от java.io:

**Блокирующий и неблокирующий режим:** 

java.nio предоставляет неблокирующие операции ввода-вывода, позволяя одному потоку эффективно обрабатывать несколько каналов. 
Это отличается от java.io, где большинство операций являются блокирующими.

**Каналы и селекторы:** 

java.nio предоставляет каналы и селекторы для многоканальной обработки, что позволяет создавать более эффективные и масштабируемые приложения ввода-вывода.

**Буферы:** 

java.nio использует буферы для эффективного обмена данными между каналами и приложением. 
Это предоставляет более гибкий и мощный механизм работы с данными.

**FileChannel:** 

В java.nio появляется новый тип канала - FileChannel, который предоставляет методы для работы с файлами, включая неблокирующее чтение и запись.

>Использование java.nio часто оправдано, когда требуется обработка множества каналов или когда необходимо обеспечить высокую производительность ввода-вывода.