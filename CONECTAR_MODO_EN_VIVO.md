# Conectar el "Modo en vivo" (tipo Kahoot) — una sola vez

El Reto Unadista ya funciona en Netlify tal cual. Pero el **Modo en vivo**
(donde varios participantes responden desde su celular en tiempo real y ves
el marcador en el proyector) necesita una conexión en tiempo real gratuita
de Google llamada **Firebase**. Se configura una sola vez, en unos 5 minutos.

Cuando termines, tendrás un "bloque de configuración" que pegarás dentro
del propio aplicativo (te lo pide la primera vez que entras al Modo en vivo).

---

## Paso 1 — Crear el proyecto en Firebase

1. Entra a **https://console.firebase.google.com** e inicia sesión con tu
   cuenta de Google (la misma del Google Sheets sirve).
2. Clic en **Crear un proyecto** (o "Agregar proyecto").
3. Ponle un nombre, por ejemplo: **reto-unadista**. Clic en Continuar.
4. Te pregunta por Google Analytics: puedes **desactivarlo** (no lo
   necesitas). Clic en Crear proyecto y espera unos segundos.

## Paso 2 — Activar la base de datos en tiempo real

1. En el menú de la izquierda, entra a **Compilación → Realtime Database**
   (Realtime Database, NO "Firestore").
2. Clic en **Crear base de datos**.
3. Ubicación: deja la que sugiere (por ejemplo Estados Unidos). Continuar.
4. Reglas de seguridad: elige **Iniciar en modo de prueba** y clic en
   Habilitar. (El modo de prueba permite leer y escribir durante unas
   semanas, suficiente para tus eventos. Más abajo te explico cómo
   renovarlo si lo sigues usando.)

## Paso 3 — Obtener el bloque de configuración

1. Arriba a la izquierda, clic en el engranaje ⚙ → **Configuración del
   proyecto**.
2. Baja hasta la sección **Tus apps** y clic en el ícono **</>** (Web).
3. Ponle un apodo, por ejemplo **reto-web**, y clic en **Registrar app**
   (no marques "Firebase Hosting", no hace falta).
4. Te mostrará un bloque de código. Lo único que necesitas es el objeto que
   se ve así:

   ```
   const firebaseConfig = {
     apiKey: "AIza...................",
     authDomain: "reto-unadista.firebaseapp.com",
     databaseURL: "https://reto-unadista-default-rtdb.firebaseio.com",
     projectId: "reto-unadista",
     storageBucket: "reto-unadista.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abcdef123456"
   };
   ```

5. **Copia desde la llave `{` hasta la llave `}`** (puedes copiar todo el
   bloque, el aplicativo entiende ambos).

   ⚠️ Importante: debe aparecer la línea **databaseURL**. Si no aparece, es
   porque no activaste la Realtime Database (vuelve al Paso 2).

## Paso 4 — Pegar la configuración en el aplicativo

1. Abre el Reto Unadista (en Netlify) y entra a **Modo en vivo — Proyectar**.
2. La primera vez te pedirá la configuración: pega ahí el bloque que
   copiaste y clic en **Guardar y continuar**.
3. ¡Listo! Queda guardado en ese equipo. No te lo volverá a pedir.

---

## Cómo se usa el día del evento

1. **Tú (director), desde el computador del proyector:** entra a
   **Modo en vivo — Proyectar**. Aparece un **PIN de 4 dígitos** en pantalla
   grande.
2. **Los participantes, desde su celular:** entran a la misma página web
   (tu enlace de Netlify), eligen **Modo en vivo — Unirme**, escriben el
   **PIN** y su **nombre**. Verás cómo aparecen conectados en la pantalla.
3. Cuando estén todos, pulsas **Comenzar reto**. Cada pregunta se proyecta
   con su temporizador; los participantes responden desde el celular tocando
   el color/figura. Entre pregunta y pregunta se muestra el **marcador**.
4. Al final se muestra el **podio con los ganadores** y los premios.

Puntos importantes:
- Todos (tú y los participantes) deben tener **internet** durante el juego en
  vivo.
- Los participantes solo entran a **tu enlace de Netlify** — no tienen que
  instalar nada ni crear cuentas.
- El **Reto individual** (el de siempre) sigue funcionando sin Firebase ni
  internet, por si quieres tener también una tablet en el stand.

---

## Renovar las reglas (si sigues usando el modo en vivo después de unas semanas)

El "modo de prueba" de Firebase caduca a las pocas semanas. Si vas a seguir
usando el modo en vivo más adelante y deja de funcionar, entra a
**Realtime Database → Reglas** y reemplaza el contenido por esto, que lo deja
abierto solo para las salas del reto:

```json
{
  "rules": {
    "salas": {
      ".read": true,
      ".write": true
    }
  }
}
```

Clic en **Publicar**. (Esto es suficiente para un evento como Casa Abierta;
las salas se borran solas al terminar cada reto.)

---

## ¿Y si no quiero usar el modo en vivo?

No pasa nada: el aplicativo funciona igual. El **Reto individual** no necesita
Firebase. El modo en vivo solo se activa si entras a "Proyectar" o "Unirme".
