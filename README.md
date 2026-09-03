# The Barbershop Leandro — sitio web

Web estática de una sola página (`index.html`), con reservas, cuentas de
cliente, panel del dueño y reseñas conectados a Supabase.

## Publicar en GitHub + Vercel

1. Crea un repositorio nuevo en GitHub (puede ser privado).
2. Sube **solo** el archivo `index.html` a la raíz del repositorio
   (no hace falta nada más: ni carpetas, ni `package.json`, ni configuración).
3. Ve a [vercel.com/new](https://vercel.com/new), conecta tu cuenta de
   GitHub e importa ese repositorio.
4. Vercel lo detectará como sitio estático automáticamente. Dale a
   **Deploy** sin tocar ninguna otra opción.
5. En un par de minutos tendrás una URL pública (tipo
   `thebarbershopleandro.vercel.app`) — pruébala ahí, no en el visor de Claude.

No hace falta configurar ninguna variable de entorno: las claves de
Supabase ya están dentro del propio `index.html` (son públicas por diseño,
la seguridad real la dan las políticas de la base de datos).

## Antes de publicar — checklist de Supabase

Ejecuta estos scripts SQL, en este orden, en Supabase → SQL Editor
(todos son seguros de volver a ejecutar si tienes dudas):

1. Migración base (perfiles, citas, reseñas) — ya la ejecutaste.
2. Migración de horarios (`business_hours`, `day_overrides`) — ya la
   ejecutaste.
3. Migración de servicios y galería — ya la ejecutaste.
4. Migración de seguridad (cierra los avisos del Security Advisor) — ya
   la ejecutaste.
5. **`supabase_migracion_resenas.sql`** (nuevo) — importa las 23 reseñas
   reales.

Y confirma que la cuenta del dueño existe y está activa:
`leandro_leo_3@hotmail.com`, contraseña `000000`, con `is_owner = true`
en la tabla `profiles` (usa `diagnostico_dueno.sql` si tienes dudas).

## Qué se arregló en esta última revisión

- **Fallo de fecha por huso horario**: la web calculaba "hoy" usando hora
  UTC en vez de hora local. En España, durante la primera hora o dos tras
  medianoche, esto podía hacer que la web pensara que seguía siendo el
  día anterior. Corregido en las 4 funciones que lo usaban.
- **23 reseñas reales** importadas con sus fechas, servicio y comentario
  originales.
- Cada botón de reserva, calendario, cuenta y panel del dueño se
  verificó ejecutando la página en un navegador simulado real (no en el
  visor de Claude), simulando tanto fallos de red como un backend que
  responde correctamente — sin errores en ningún caso.
