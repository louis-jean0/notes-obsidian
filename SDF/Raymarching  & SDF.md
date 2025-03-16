## Basic raymarching

SDF provides, at any given point, the minimum distance to the nearest object in the scene. This property enables raymarching : starting from the camera's position, we cast a ray through every pixel. The ray direction is taken directly from the uv coordinates of the quad that covers the window, ensuring each pixel is addressed. At each step along the ray, we evaluate the SDF; the returned distance tells us how far we can safely advance without overshooting a surface. If this distance falls below a set threshold (for example, 0.001), we conclude that we are nearly touching a surface and stop. Similarly, if the cumulative distance traveled exceeds a predefined limit, we terminate the loop, indicating no object was encountered along that ray. Additionally, imposing a maximum number of steps guarantees that the loop will exit. This technique is efficient because it adapts the stepping distance based on the SDF's feedback, avoiding unnecessary calculations.

![[image_raymarching.png]]

```glsl
float ray_march(vec3 ray_origin, vec3 ray_direction, float max_dist_to_travel) {
    float distance_on_ray = 0.0;
    for(int i = 0; i < NB_STEPS; ++i) {
        vec3 current_position = ray_origin + ray_direction * distance_on_ray;
        float distance_to_sdf = map(current_position);
        if(distance_to_sdf < MIN_DIST_TO_SDF) {
            break;
        }
        distance_on_ray += distance_to_sdf;
        if(distance_on_ray > MAX_DIST_TO_TRAVEL) {
            break;
        }

    }
    return distance_on_ray;
}
```

