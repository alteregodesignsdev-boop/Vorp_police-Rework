# Vorp_police-Rework
This is a reworked version of the original vorp_police resource, created to improve flexibility and server customization.

🔧 New Features:

Added a custom rank system, fully configurable through the config.lua.

Allows server owners to define and manage police ranks directly from the configuration file.

Designed to maintain full compatibility with the VorpCore framework.

💡 Built with performance, customization, and server immersion in mind.

# Sistema de Rangos para vorp_police

## Descripción
Este sistema permite crear rangos jerárquicos dentro de cada trabajo policial con permisos específicos para cada rango.

## Características Principales

### 1. Configuración de Rangos
- **6 rangos por trabajo** (grados 0-5)
- **Permisos específicos** para cada rango
- **Nombres y etiquetas personalizables**
- **Sistema jerárquico** (rangos superiores pueden gestionar inferiores)

### 2. Permisos Disponibles
- `canJail`: Puede encarcelar jugadores
- `canHire`: Puede contratar jugadores
- `canFire`: Puede despedir jugadores
- `canUseCuffs`: Puede usar esposas
- `canDrag`: Puede arrastrar jugadores
- `canAlert`: Puede recibir alertas de ciudadanos

### 3. Comandos Nuevos
- `/promover [ID] [grado]`: Promover o degradar jugadores
- `/rangos`: Ver información de rangos y permisos
- `/verrango [ID]`: Ver el rango de otro jugador

## Configuración

### Estructura de Rangos en config.lua
```lua
Config.JobRanks = {
    ["BWPolice"] = {
        [0] = { name = "Cadete", label = "Cadete de Policía", canJail = false, canHire = false, canFire = false, canUseCuffs = true, canDrag = true, canAlert = true },
        [1] = { name = "Oficial", label = "Oficial de Policía", canJail = true, canHire = false, canFire = false, canUseCuffs = true, canDrag = true, canAlert = true },
        -- ... más rangos
    }
}
```

### Funciones Helper
- `Config.GetPlayerRank(job, grade)`: Obtiene información del rango
- `Config.HasPermission(job, grade, permission)`: Verifica permisos

## Integración

### Verificación de Permisos
```lua
-- Servidor
if hasRankPermission(user, "jail") then
    -- Permitir encarcelar
end

-- Cliente (usando callback)
local hasPermission = Core.Callback.TriggerAwait("vorp_police:server:hasPermission", "hire")
```

### Eventos Actualizados
- `vorp_police:server:hirePlayer`: Ahora incluye parámetro de grado
- `vorp_police:server:firePlayer`: Verifica jerarquía de rangos
- Todas las funciones de esposas, arrastrar y cárcel verifican permisos

## Trabajos Configurados

### Rangos por Defecto (0-5)
1. **Grado 0**: Cadete - Permisos básicos (esposas, arrastrar, alertas)
2. **Grado 1**: Oficial - + Encarcelar
3. **Grado 2**: Sargento - + Contratar
4. **Grado 3**: Teniente - + Despedir
5. **Grado 4**: Capitán - Todos los permisos
6. **Grado 5**: Jefe - Todos los permisos (máximo rango)

### Trabajos Incluidos
- BWPolice (Blackwater)
- RhoSheriff (Rhodes)
- SDPolice (Saint Denis)
- StrSheriff (Strawberry)
- ArmSheriff (Armadillo)
- ValSheriff (Valentine)

## Uso

### Para Administradores
1. Configurar rangos en `config.lua`
2. Asignar grados iniciales al contratar
3. Usar `/promover` para gestionar rangos

### Para Jugadores
1. Usar `/rangos` para ver permisos actuales
2. Usar `/verrango [ID]` para verificar rangos de otros
3. Los permisos se aplican automáticamente según el rango

## Archivos Modificados
- `configs/config.lua`: Configuración de rangos
- `server/main.lua`: Lógica de permisos
- `server/rank_management.lua`: Comandos de gestión (NUEVO)
- `client/main.lua`: Interfaz de contratación
- `fxmanifest.lua`: Inclusión del nuevo archivo

## Compatibilidad
- Compatible con la versión existente de vorp_police
- No requiere cambios en la base de datos
- Los trabajos existentes mantienen funcionalidad básica
