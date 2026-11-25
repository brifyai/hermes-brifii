# hermes-brifii

Stack Docker de Hermes Agent + WebUI para EasyPanel (VPS 96GB).

## Deploy en EasyPanel

1. **+ Servicio → Compose** con fuente Git: este repo, rama main
2. **Environment**: copia `.env.example` y llena los valores reales (mismos de la VPS vieja)
3. **Dominios**: apunta el subdominio de la WebUI al puerto 8787

⚠️ El volumen `data` es `external` y usa `brifii_noborrar_hermes-agent-brifii_data`
(el volumen que ya tiene la identidad migrada: skills, memories, config.yaml, SOUL.md).

## Runbook post-deploy

Cada **Implementar** recrea el contenedor de hermesui y borra su venv. Re-ejecutar:

```bash
HE=$(docker ps --format '{{.Names}}' | grep -iE 'hermesui' | head -1)
docker exec $HE /app/venv/bin/pip install -e /opt/data/hermes-agent --quiet
docker restart $HE
```

## Verificación de identidad

- "¿qué modelo usa producción?" → DeepSeek deepseek-flash
- "¿cuál es la regla dura con los fiscales?" → 1 intento sin reintentos

## Seguridad

- GLM solo desarrollo — producción usa DeepSeek
- `.env` jamás al repo
