# Reservas Deportivas

Aplicación móvil Expo/React Native para consultar espacios y disponibilidad, gestionar reservas oficiales y facilitar el trabajo del personal.

## Requisitos

- Node.js 22.13 o superior (Expo SDK 58).
- Una URL de la API accesible desde el dispositivo. `localhost` apunta al propio teléfono cuando se usa un APK.
- Para generar un APK en la nube: una cuenta de Expo/EAS y el proyecto vinculado.

## Configuración local

1. Instalar dependencias:

   ```bash
   npm install
   ```

2. Copiar `.env.example` a `.env.local` y configurar `EXPO_PUBLIC_API_URL` con la URL base real de la API, sin una barra final. Para probar desde un teléfono, usar una dirección HTTPS pública o la IP accesible de la computadora en la misma red.

3. Iniciar Expo:

   ```bash
   npx expo start
   ```

La aplicación no usa reservas simuladas cuando la API no está disponible. En ese caso muestra un error o, para lecturas admitidas, la última copia local; las escrituras requieren confirmación del servidor.

## APK de preview

El perfil `preview` de `eas.json` genera un APK de distribución interna:

```bash
npx eas-cli@latest login
npx eas-cli@latest init
npx eas-cli@latest build --platform android --profile preview
```

Antes de iniciar el build:

- Configurar `EXPO_PUBLIC_API_URL` como variable de entorno **preview** del proyecto EAS, con la misma URL accesible desde el teléfono. Las variables `EXPO_PUBLIC_*` quedan incluidas en el bundle de la app; no guardar secretos allí.
- Vincular el proyecto EAS y configurar `EXPO_PUBLIC_EAS_PROJECT_ID` con el `projectId` resultante como variable **preview**. Se usa para solicitar tokens push; las notificaciones push requieren además credenciales de Expo/FCM.
- El identificador Android/iOS `com.reservasdeportivas.app` está puesto como valor inicial para el preview. Confirmar/cambiarlo por el identificador controlado por el titular antes de publicar o distribuir como versión final.

El APK queda disponible en el enlace que devuelve EAS al finalizar el build. La API de referencia todavía responde `501 NO_IMPLEMENTADO` al registro de dispositivos push; las notificaciones locales de reservas no dependen de ese módulo.

## Comprobaciones

```bash
npm run lint
npx tsc --noEmit
```
