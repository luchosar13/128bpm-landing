# ADR 0001 · Flujo de login y compra

- **Estado:** aceptado por Luciano el 25/11/2026 (pendiente de validar con el cliente los puntos abiertos)
- **Contexto:** el flujo de la propuesta v1.0 (formulario → email para crear cuenta → WhatsApp → plan → pago) demora el primer pago horas o días. El cliente pidió un flujo más rápido para que el comprador no se enfríe.
- **Comparativa:** https://claude.ai/artifact/TcNpmdny1uhfeJGkn8uEW2 (escenarios A, B y C).

## Decisión

**Escenario A para comprar + login del escenario B para volver a entrar.**

1. El visitante elige paquete, salida y plan de cuotas en una sola pantalla.
2. Deja nombre, email y WhatsApp (sin crear cuenta) y paga la seña con el Payment Brick de Mercado Pago, embebido en el sitio.
3. Con el pago aprobado por webhook, la reserva queda confirmada y el plan de cuotas se genera automáticamente. El equipo lo puede ajustar desde el panel.
4. La cuenta del viajero se crea con ese email y se activa con Google o con link mágico. Reservar no da acceso al perfil.
5. Login de clientes que vuelven: código de 6 dígitos por email o Google (Supabase Auth). Sin contraseñas.
6. El contacto por WhatsApp pasa a ser posterior al pago.

## Consecuencias

- **Cambio de alcance** respecto de la propuesta v1.0 (secciones 04, 5.2, 5.3 y 5.4): seña al reservar, plan automático, reserva confirmada al pagar, se elimina el login con contraseña. Se cotiza aparte (sección 09).
- Sin costos nuevos de terceros: Supabase Pro, Resend y Mercado Pago ya están previstos.
- Seguridad: monto calculado en el servidor, cupo retenido con vencimiento y restricción en base de datos, webhook con firma verificada e idempotencia, CAPTCHA invisible y límites de intentos en formulario y códigos.

## Pendientes antes de implementar

- [ ] Monto o porcentaje de la seña y moneda de cobro (SUPUESTO en mockups: USD 300 cobrados en pesos).
- [ ] Tiempo de retención del cupo durante el pago (SUPUESTO: 15 minutos).
- [ ] Regla del saldo: cuotas mensuales iguales hasta X días antes de la salida (SUPUESTO: 30 días).
- [ ] Validación legal: devolución de la seña dentro de los 10 días (Ley 24.240 art. 34, Res. SCI 424/2020) y Política de Cancelación.
- [ ] Validación contable: percepciones sobre servicios turísticos al exterior cobrados en pesos.
- [ ] Aprobación del cambio de alcance y su cotización.
