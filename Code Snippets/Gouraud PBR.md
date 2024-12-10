
### Pure Vertex

```
shader_type spatial;
render_mode unshaded, skip_vertex_transform;

uniform sampler2D tex : filter_nearest;  // Texture with nearest filtering
global uniform vec4 light_color;
global uniform vec3 light_pos;  // Light position in world space
global uniform float ambient;
global uniform vec4 ambient_color;

varying vec3 lighting;

float fresnelSchlick(float cosTheta, float F0) {
    cosTheta = clamp(cosTheta, 0.001, 1.0);  // Avoid exactly zero or negative values
    return F0 + (1.0 - F0) * pow(1.0 - cosTheta, 5.0);
}

// GGX / Trowbridge-Reitz Normal Distribution Function
float D_GGX(float NdotH, float roughness) {
    float alpha = roughness * roughness;
    float alpha2 = alpha * alpha;
    float denom = (NdotH * NdotH) * (alpha2 - 1.0) + 1.0;
    return alpha2 / (PI * denom * denom);
}

// Geometry function using Smith's Schlick-GGX
float G_SchlickGGX(float NdotV, float roughness) {
    float k = (roughness + 1.0) * (roughness + 1.0) / 8.0;
    return NdotV / (NdotV * (1.0 - k) + k);
}


// Cook-Torrance BRDF
vec3 CookTorranceSpecular(vec3 lightColor, float NdotL, float NdotV, float NdotH, float VdotH, float roughness, float F0) {
    // Fresnel term (Schlick approximation)
    float F = fresnelSchlick(VdotH, F0);

    // Normal distribution function (GGX)
    float D = D_GGX(NdotH, roughness);

    // Geometry function (Smith's Schlick-GGX)
    float G = G_SchlickGGX(NdotV, roughness) * G_SchlickGGX(NdotL, roughness);

    // Cook-Torrance denominator
    float denom = 4.0 * NdotV * NdotL + 0.001; // Avoid division by zero

    // Final Cook-Torrance specular term
    return (D * G * F) / denom * lightColor;
}

void vertex() {
	float metallic = COLOR.r;
	float roughness = COLOR.g;

    // Step 1: Transform the vertex and normals to world space
    vec3 world_position = (MODEL_MATRIX * vec4(VERTEX, 1.0)).xyz;
    vec3 world_normal = normalize((MODEL_MATRIX * vec4(NORMAL, 0.0)).xyz);

	// Calculate view direction in world space
    vec3 viewDir = normalize(CAMERA_POSITION_WORLD - world_position);
    // Calculate light direction in world space
    vec3 lightDir = normalize(light_pos - world_position);
	// Compute the half-vector for the Cook-Torrance model
    vec3 halfDir = normalize(viewDir + lightDir);

	// Fresnel reflectance at normal incidence
    float F0 = mix(0.04, 1.0, metallic);  // 0.04 for dielectric, 1.0 for conductor (fully metallic)

    // Relevant dot products
    float NdotV = max(dot(world_normal, viewDir), 0.0);
    float NdotL = max(dot(world_normal, lightDir), 0.0);
    float NdotH = max(dot(world_normal, halfDir), 0.0);
    float VdotH = max(dot(viewDir, halfDir), 0.0);

	// Cook-Torrance specular calculation
    vec3 specular = CookTorranceSpecular(light_color.rgb, NdotL, NdotV, NdotH, VdotH, roughness, F0);

    // Diffuse term using Lambertian reflectance
    float diffuse = (1.0 - metallic) * NdotL;

	// Calculate ambient component
	vec3 ambient_component = ambient_color.rgb * ambient;

    // Compute the final color (pre-computed)
	lighting = (1.0 - metallic) * diffuse * light_color.rgb + specular + ambient_component;

    VERTEX = (VIEW_MATRIX * vec4(world_position, 1.0)).xyz;
}

void fragment() {
    // Sample the base texture color
    vec3 tex_color = texture(tex, UV).rgb;

    // Calculate final diffuse color affected by lighting
    vec3 lit_color = tex_color * lighting;

    // Emission component for self-illumination
    vec3 emission_component = tex_color * COLOR.b;

    // Combine the diffuse and emission components
    ALBEDO = lit_color + emission_component;
}

```

### Lerped Lighting

```
shader_type spatial;
render_mode unshaded, skip_vertex_transform;

uniform sampler2D tex : filter_nearest;  // Texture with nearest filtering
uniform float mat_roughness: hint_range(0.0, 1.0);
uniform float mat_metallic: hint_range(0.0, 1.0);
global uniform vec4 light_color;
global uniform vec3 light_pos;  // Light position in world space
global uniform float ambient;
global uniform vec4 ambient_color;

varying float frag_NdotV;
varying float frag_NdotL;
varying float frag_NdotH;
varying float frag_VdotH;
varying float frag_F0;

float fresnelSchlick(float cosTheta, float F0) {
    cosTheta = clamp(cosTheta, 0.001, 1.0);  // Avoid exactly zero or negative values
    return F0 + (1.0 - F0) * pow(1.0 - cosTheta, 5.0);
}

// GGX / Trowbridge-Reitz Normal Distribution Function
float D_GGX(float NdotH, float roughness) {
    float alpha = roughness * roughness;
    float alpha2 = alpha * alpha;
    float denom = (NdotH * NdotH) * (alpha2 - 1.0) + 1.0;
    return alpha2 / (PI * denom * denom);
}

// Geometry function using Smith's Schlick-GGX
float G_SchlickGGX(float NdotV, float roughness) {
    float k = (roughness + 1.0) * (roughness + 1.0) / 8.0;
    return NdotV / (NdotV * (1.0 - k) + k);
}


// Cook-Torrance BRDF
vec3 CookTorranceSpecular(vec3 lightColor, float NdotL, float NdotV, float NdotH, float VdotH, float roughness, float F0) {
    // Fresnel term (Schlick approximation)
    float F = fresnelSchlick(VdotH, F0);

    // Normal distribution function (GGX)
    float D = D_GGX(NdotH, roughness);

    // Geometry function (Smith's Schlick-GGX)
    float G = G_SchlickGGX(NdotV, roughness) * G_SchlickGGX(NdotL, roughness);

    // Cook-Torrance denominator
    float denom = 4.0 * NdotV * NdotL + 0.001; // Avoid division by zero

    // Final Cook-Torrance specular term
    return (D * G * F) / denom * lightColor;
}

void vertex() {
    // Step 1: Transform the vertex and normals to world space
    vec3 world_position = (MODEL_MATRIX * vec4(VERTEX, 1.0)).xyz;
    vec3 world_normal = normalize((MODEL_MATRIX * vec4(NORMAL, 0.0)).xyz);

	// Calculate view direction in world space
    vec3 viewDir = normalize(CAMERA_POSITION_WORLD - world_position);
    // Calculate light direction in world space
    vec3 lightDir = normalize(light_pos - world_position);
	// Compute the half-vector for the Cook-Torrance model
    vec3 halfDir = normalize(viewDir + lightDir);

    //// Relevant dot products
    frag_NdotV = max(dot(world_normal, viewDir), 0.0);
    frag_NdotL = max(dot(world_normal, lightDir), 0.0);
    frag_NdotH = max(dot(world_normal, halfDir), 0.0);
    frag_VdotH = max(dot(viewDir, halfDir), 0.0);
	frag_F0 = mix(0.04, 1.0, mat_metallic);  // 0.04 for dielectric, 1.0 for conductor (fully metallic)


    VERTEX = (VIEW_MATRIX * vec4(world_position, 1.0)).xyz;
	NORMAL = normalize((MODELVIEW_MATRIX * vec4(NORMAL, 0.0)).xyz);
}

void fragment() {
    // Sample the base texture color
    vec3 tex_color = texture(tex, UV).rgb;

	float metallic = mat_metallic;
	float roughness = mat_roughness;
	
    vec3 specular = CookTorranceSpecular(light_color.rgb, frag_NdotL, frag_NdotV, frag_NdotH, frag_VdotH, roughness, frag_F0);

	 // Diffuse term using Lambertian reflectance
    float diffuse = (1.0 - metallic) * frag_NdotL;
	//float diffuseScale = 1.0 - fresnelSchlick(1.0, frag_F0); // Fresnel at grazing angles
	//float diffuse = (1.0 - metallic) * frag_NdotL * diffuseScale;

	// Calculate ambient component
	vec3 ambient_component = ambient_color.rgb * ambient;

    // Compute the final color (pre-computed)
	vec3 finalColor = (1.0 - metallic) * diffuse * light_color.rgb + specular + ambient_component;

    // Calculate final diffuse color affected by lighting
    vec3 lit_color = tex_color * finalColor;

    ALBEDO = lit_color;
}

```