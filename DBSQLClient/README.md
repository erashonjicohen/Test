#OtherInformation: <Nullable>enable</Nullable>
[]: # OtherInformation: <ImplicitUsings>enable</ImplicitUsings>
```csharp 
# Configuración avanzada para formateo salida del JSON (JsonSerializerOptions) 
# ============================================


var options = new JsonSerializerOptions
{
    // ============================================
    // FORMATO Y PRESENTACIÓN
    // ============================================
    
    // Formato con indentación (más legible)
    WriteIndented = true,  // Por defecto: true en tu clase
    
    // Tamaño de la indentación
    IndentCharacter = ' ',  // Carácter para indentar (espacio por defecto)
    IndentSize = 2,         // Número de caracteres (2 espacios por defecto)
    
    // ============================================
    // NOMENCLATURA DE PROPIEDADES
    // ============================================
    
    // Convierte nombres de propiedades
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,  // "Name" → "name"
    // Opciones: null (sin cambios), JsonNamingPolicy.CamelCase, JsonNamingPolicy.SnakeCaseLower, JsonNamingPolicy.SnakeCaseUpper, JsonNamingPolicy.KebabCaseLower, JsonNamingPolicy.KebabCaseUpper

    
    // Convierte nombres de diccionarios
    DictionaryKeyPolicy = JsonNamingPolicy.CamelCase,
    
    // ============================================
    // MANEJO DE VALORES
    // ============================================
    
    // Ignora propiedades null al escribir
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull,
    // Opciones:
    // - Never: Incluye todo
    // - Always: Ignora todo
    // - WhenWritingNull: Ignora solo null
    // - WhenWritingDefault: Ignora valores por defecto (0, false, null, etc.)
    
    // Permite comentarios en JSON al leer
    ReadCommentHandling = JsonCommentHandling.Skip,
    
    // Permite comas finales
    AllowTrailingCommas = true,
    
    // ============================================
    // NÚMEROS Y TIPOS ESPECIALES
    // ============================================
    
    // Permite leer números como strings
    NumberHandling = JsonNumberHandling.AllowReadingFromString,
    // Opciones:
    // - Strict: Solo números
    // - AllowReadingFromString: "123" → 123
    // - WriteAsString: 123 → "123"
    // - AllowNamedFloatingPointLiterals: Permite NaN, Infinity
    
    // ============================================
    // CASE SENSITIVITY
    // ============================================
    
    // Ignora mayúsculas/minúsculas al deserializar
    PropertyNameCaseInsensitive = true,  // Por defecto: true en tu clase
    
    // ============================================
    // ENCODING Y CARACTERES
    // ============================================
    
    // Codifica caracteres HTML (<, >, &, ', ")
    Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping,
    // Opciones:
    // - JavaScriptEncoder.Default: Codifica todo
    // - JavaScriptEncoder.UnsafeRelaxedJsonEscaping: Más permisivo
    
    // ============================================
    // PROFUNDIDAD Y TAMAÑO
    // ============================================
    
    // Profundidad máxima de objetos anidados
    MaxDepth = 64,  // Por defecto: 64
    
    // Tamaño máximo del buffer
    DefaultBufferSize = 16384,  // 16KB por defecto
    
    // ============================================
    // REFERENCIAS Y CICLOS
    // ============================================
    
    // Manejo de referencias circulares
    ReferenceHandler = ReferenceHandler.IgnoreCycles,
    // Opciones:
    // - null: Error en ciclos
    // - ReferenceHandler.Preserve: Usa $id y $ref
    // - ReferenceHandler.IgnoreCycles: Ignora referencias circulares
    
    // ============================================
    // CONVERSORES PERSONALIZADOS
    // ============================================
    
    // Agregar conversores personalizados
    Converters = 
    {
        new JsonStringEnumConverter(),  // Convierte enums a strings
        // Puedes agregar tus propios conversores aquí
    },
    
    // ============================================
    // OTRAS OPCIONES
    // ============================================
    
    // Permite campos además de propiedades
    IncludeFields = false,
    
    // Ignora propiedades de solo lectura al serializar
    IgnoreReadOnlyProperties = false,
    
    // Ignora propiedades de solo lectura al deserializar
    IgnoreReadOnlyFields = false,
    
    // Orden de las propiedades
    PreferredObjectCreationHandling = JsonObjectCreationHandling.Replace,
    
    // Respeta [Required] attributes
    RespectRequiredConstructorParameters = false,
    
    // Respeta atributos de validación
    RespectNullableAnnotations = false,
    
    // Permite referencias no resueltas
    UnmappedMemberHandling = JsonUnmappedMemberHandling.Skip,
    
    // Comportamiento de strings desconocidas en enums
    UnknownTypeHandling = JsonUnknownTypeHandling.JsonElement
};



// ============================================
// Ejemplo 1: JSON Compacto (sin indentación)
// ============================================
var compactOptions = new JsonSerializerOptions
{
    WriteIndented = false
};
string compact = result.ToJson(compactOptions);
// Resultado: [{"Id":1,"Name":"Juan"},{"Id":2,"Name":"María"}]

// ============================================
// Ejemplo 2: CamelCase (estilo JavaScript)
// ============================================
var camelCaseOptions = new JsonSerializerOptions
{
    WriteIndented = true,
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase
};
string camelJson = result.ToJson(camelCaseOptions);
/* Resultado:
[
  {
    "id": 1,
    "name": "Juan",
    "email": "juan@email.com"
  }
]
*/

// ============================================
// Ejemplo 3: Incluir valores null y por defecto
// ============================================
var includeAllOptions = new JsonSerializerOptions
{
    WriteIndented = true,
    DefaultIgnoreCondition = JsonIgnoreCondition.Never
};
string withNulls = result.ToJson(includeAllOptions);
/* Resultado:
[
  {
    "Id": 1,
    "Name": "Juan",
    "Email": null,
    "IsActive": false
  }
]
*/

// ============================================
// Ejemplo 4: Manejo especial de números
// ============================================
var numberOptions = new JsonSerializerOptions
{
    WriteIndented = true,
    NumberHandling = JsonNumberHandling.WriteAsString
};
string numbersAsStrings = result.ToJson(numberOptions);
/* Resultado:
[
  {
    "Id": "1",
    "Price": "99.99",
    "Name": "Producto"
  }
]
*/

// ============================================
// Ejemplo 5: Manejo de referencias circulares
// ============================================
var cyclicOptions = new JsonSerializerOptions
{
    WriteIndented = true,
    ReferenceHandler = ReferenceHandler.IgnoreCycles
};
string safeJson = result.ToJson(cyclicOptions);

// ============================================
// Ejemplo 6: Caracteres especiales sin codificar
// ============================================
var relaxedOptions = new JsonSerializerOptions
{
    WriteIndented = true,
    Encoder = System.Text.Encodings.Web.JavaScriptEncoder.UnsafeRelaxedJsonEscaping
};
string unescaped = result.ToJson(relaxedOptions);
// "Nombre": "José & María" (en lugar de "Jos\u00E9 & Mar\u00EDa")

// ============================================
// Ejemplo 7: Configuración para APIs REST
// ============================================
var apiOptions = new JsonSerializerOptions
{
    WriteIndented = false,  // Compacto para red
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,  // Estándar JavaScript
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull,  // Menos datos
    PropertyNameCaseInsensitive = true,  // Flexible en lectura
    Converters = { new JsonStringEnumConverter() }  // Enums legibles
};
string apiJson = result.ToJson(apiOptions);

// ============================================
// Ejemplo 8: Configuración para logs/debug
// ============================================
var debugOptions = new JsonSerializerOptions
{
    WriteIndented = true,  // Legible
    DefaultIgnoreCondition = JsonIgnoreCondition.Never,  // Ver todo
    ReferenceHandler = ReferenceHandler.IgnoreCycles,  // Evitar errores
    MaxDepth = 10  // Limitar profundidad
};
string debugJson = result.ToJson(debugOptions);






// ============================================
// Configuración por defecto reutilizable
private static readonly JsonSerializerOptions DefaultJsonOptions = new JsonSerializerOptions
{
    PropertyNameCaseInsensitive = true,  // Flexible al leer
    WriteIndented = true,  // Formato legible
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull,  // No incluir nulls
    Converters = { new JsonStringEnumConverter() }  // Enums como texto
};