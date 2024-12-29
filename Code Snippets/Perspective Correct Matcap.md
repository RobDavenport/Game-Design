```
shader_type spatial;
render_mode unshaded, skip_vertex_transform;

uniform sampler2D tex;

void vertex() {
	// View Space Normal
	NORMAL = normalize(MODELVIEW_MATRIX * vec4(NORMAL, 0.0)).xyz;
	// Clip Position
    VERTEX = (MODELVIEW_MATRIX * vec4(VERTEX, 1.0)).xyz;
}

void fragment() {
	// VIEW = View Space Vector to Fragment
	// NORMAL = View Space Normal
	vec3 viewCross = cross(VIEW, NORMAL);
	
	// Might need to adjust the -s and such here to get proper UV mapping
	vec2 matcap_uv = vec2(viewCross.y, viewCross.x) * 0.5 + 0.5;
	ALBEDO = texture(tex, matcap_uv).xyz;
}

```