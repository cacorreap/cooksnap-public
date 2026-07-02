# Suscripción por email para avisar de actualizaciones (plan)

> Idea para retomar más adelante. Objetivo: botón **"Avísame de novedades"** en la
> landing (GitHub Pages) donde el usuario deja su correo, y recibir un email cuando
> haya una **actualización grande** con el APK.

GitHub Pages es **estático** (sin backend), así que hacen falta servicios externos.
Son dos partes: **(1) recolectar** el correo y **(2) enviar** el aviso.
**Regla de oro:** no enviar desde tu Gmail/SMTP propio → te marcan spam. Usar un ESP
(servicio de email) que maneja unsubscribe, doble opt-in y entregabilidad.

## Enfoque recomendado (simple, $0): servicio de newsletter
1. Crear cuenta en un ESP con free tier:
   - **MailerLite** — gratis ~1.000 suscriptores **con envío** incluido (recomendado).
   - **Buttondown** — gratis ~100 subs, dev-friendly, buena API + RSS.
   - **Mailchimp** — gratis ~500 contactos, soporta campañas RSS.
2. Poner su **formulario embebido** (o un `<form>` a su endpoint) como botón
   **"Avísame de novedades"** en `index.html`. El correo queda en su panel.
3. En cada **actualización grande**: entrar al panel y mandar un **broadcast** manual.
   → Tú decides qué es "grande". $0, ~30 min de setup.

## Opción automática (email por cada release)
GitHub expone un **feed RSS de releases**:
`https://github.com/cacorreap/cooksnap-public/releases.atom`

- **RSS-to-email** del ESP (Buttondown / Mailchimp): conectar ese feed → cada release
  envía solo. Cero código.
- **GitHub Action**: al publicar un Release, un workflow llama a la API del ESP y
  dispara el correo. Archivo en `.github/workflows/notify-subscribers.yml`, con la
  API key del ESP como **secreto** del repo.

⚠️ Lo automático manda email en **cada** release (ruidoso). Para "solo grandes":
dejarlo **manual**, o automatizar solo tags major (`vX.0.0`).

## Qué NO hacer
- Recolectar con un form "a tu correo" (Formspree, etc.) y enviar tú mismo → spam, no escala.
- Guardar correos en el repo (¡son públicos!). Los correos viven en el ESP.

## Costos / límites
Free tiers alcanzan para cientos/miles de suscriptores. Solo se paga al crecer mucho.

## Checklist para implementar (cuando se retome)
- [ ] Elegir ESP y crear cuenta (sugerido: MailerLite).
- [ ] Crear el "grupo"/lista de suscriptores y activar **doble opt-in**.
- [ ] Copiar el **endpoint/embed** del formulario.
- [ ] Agregar el bloque **"Avísame de novedades"** en `index.html` (form estilizado con la paleta CookSnap).
- [ ] (Opcional) Automatizar: RSS-to-email del ESP **o** `notify-subscribers.yml` (GitHub Action) con la API key como secreto.
- [ ] Probar: suscribirse, confirmar el opt-in, y hacer un envío de prueba.

## Dificultad estimada
| Parte | Dificultad |
|---|---|
| Form + botón en la landing | Muy fácil |
| Guardar correos | Lo hace el ESP (gratis) |
| Aviso manual en updates grandes | Fácil (panel del ESP) |
| Aviso automático por release | Medio (RSS-to-email o GitHub Action) |
