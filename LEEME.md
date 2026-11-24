# Nerva · Encargos y caja

### 👉 https://rebolloagus.github.io/nerva/

App para llevar los encargos de Nerva (madera y resina): quién encargó qué, para
cuándo, cuánto dejó de seña y cuánto falta cobrar, más la caja con todas las
entradas y los gastos. Todo vive en un solo archivo: `index.html`.

Se adapta sola: en el celular queda como una app con las pestañas abajo, y en la
compu muestra los encargos en columnas.

## Cómo se usa

**Encargos**: botón *Encargo* para anotar uno nuevo:

- Cliente y teléfono. Si el cliente ya encargó antes, el teléfono se completa solo.
- Las piezas: tocando un producto de la lista se agrega con su precio, o *+ Otra
  pieza* para algo a medida. Cada pieza lleva medida, color de resina y grabado.
  El precio se puede cambiar en cada encargo sin tocar la lista de precios.
- Fecha de entrega y si retira o se envía.
- **Seña y pagos**: el botón *Seña 50%* calcula la mitad sola. Se elige si fue
  en efectivo o por transferencia y queda registrada en la caja. Se pueden
  anotar varios pagos (seña, entregas a cuenta, saldo): la barra muestra cuánto
  pagó y cuánto falta.
- *Mandar comprobante* le manda al cliente por WhatsApp el detalle con la seña
  y el saldo.

En la lista, cada encargo avanza con un toque: **Empezar → Está listo →
Entregar**. Cuando está listo aparece *Avisar*, que le escribe al cliente por
WhatsApp que ya está pronto. Al *Entregar*, si falta plata, pregunta cómo pagó
el saldo (o se entrega igual y queda en *Deben plata*).

Arriba se ve siempre cuántos encargos hay en el taller, cuántos están atrasados,
las señas cobradas de lo que falta entregar, y cuánto hay por cobrar.

**Caja**: entradas y salidas del mes.

- Las señas y saldos de los encargos entran solos.
- *+ Gasto* para madera, resina, insumos, herramientas, envíos, publicidad…
- *+ Entrada* para lo que no viene de un encargo (una venta directa, por ejemplo).
- Muestra el resultado del mes, cuánto fue en efectivo y cuánto por
  transferencia, los gastos por rubro y el acumulado desde que empezaron a
  usar la app.

**Productos**: la lista de precios. Lo que se carga acá aparece como botón al
tomar un encargo. Cambiar un precio no toca los encargos ya anotados.

## Sincronizar los celulares

Sin configurar nada la app funciona perfecto, pero **cada celular guarda lo
suyo**: si anotás un encargo en uno, no aparece en el otro. Para que todos vean
lo mismo al instante, una sola vez:

1. Entrar a **console.firebase.google.com** con la cuenta de Google del negocio y
   crear un proyecto (el plan gratis alcanza de sobra; se puede saltear Analytics).
2. Dentro del proyecto: **Compilación → Firestore Database → Crear base de datos**,
   modo producción, ubicación `southamerica-east1`.
3. En **Configuración del proyecto** (el engranaje) → *Tus apps* → ícono `</>` para
   registrar una app web (sin marcar Hosting). Firebase muestra un bloque de código:
   copiarlo entero, tal cual.
4. En la app: engranaje arriba a la derecha → pegar eso en *Configuración de
   Firebase*, poner el **código del negocio** y tocar *Conectar*. El puntito del
   header se pone verde y dice "En línea".

   **Para sumar los demás celulares no hace falta repetir esto:** en uno ya
   conectado, Ajustes → *📲 Conectar otro celular* genera un link. Quien lo abre
   queda conectado a los mismos encargos. Ese link lleva la llave del negocio, así
   que va solo a los del taller.

   El código del negocio es la llave de la base: inventá uno largo y difícil de
   adivinar, y **no lo publiques en ningún lado**. Acá va como `TU-CODIGO-SECRETO`
   justamente porque este archivo es público.
5. En **Firestore → Reglas**, pegar esto y publicar, con el código del paso 4 en
   lugar de `TU-CODIGO-SECRETO`:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /negocios/{negocio}/{documento=**} {
      allow read, write: if negocio == 'TU-CODIGO-SECRETO';
    }
  }
}
```

Sin ese último paso la base queda abierta a cualquiera.

## Instalarla en el celular

**Primero conectar la sincronización, después agregar a la pantalla de inicio.** En
el iPhone el acceso directo guarda sus datos aparte de Safari, así que si lo creás
antes de conectar, se abre vacío y dice "Local".

- **iPhone:** abrir el link **en Safari** → botón Compartir → *Añadir a pantalla de inicio*.
- **Android:** abrirlo en Chrome → los tres puntos → *Agregar a pantalla de inicio*.

Queda con el logo de Nerva y se abre en pantalla completa, como una app cualquiera.

> No la uses desde el mini-navegador que abre Instagram o WhatsApp adentro del chat:
> ese borra los datos guardados al cerrarse. Copiar el link y pegarlo en Safari o Chrome.

## Respaldo

Ajustes → *Exportar datos* baja un `.json` con todo. *Importar datos* lo vuelve a
cargar. Conviene hacerlo cada tanto, sobre todo si no usan la sincronización.
