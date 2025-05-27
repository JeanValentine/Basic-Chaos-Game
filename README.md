import matplotlib.pyplot as plt
import matplotlib.animation as animation
import random
import colorsys
import math

# Set up polygon
def init_polygon(n, radius=1):
    delta_angle = 360 / n
    polygon = []
    
    for i in range(n):
        angle = math.radians(i * delta_angle)
        x = radius * math.cos(angle)
        y = radius * math.sin(angle)
        color = colorsys.hsv_to_rgb(i / n, 1, 1)
        polygon.append(((x, y), color))
    
    return polygon

# Parameters
num_sides = 3
r = 0.5
polygon = init_polygon(num_sides)
x, y = 0, 0
points = []
colors = []

# Plot setup
fig, ax = plt.subplots(figsize=(6, 6))
sc = ax.scatter([], [], s=0.2)
ax.set_xlim(-1.1, 1.1)
ax.set_ylim(-1.1, 1.1)
ax.axis('off')
ax.set_title(f'Chaos Game Live (n={num_sides})')

def update(frame):
    global x, y
    for _ in range(50):  # Add multiple points per frame for speed
        vertex, color = random.choice(polygon)
        x += (vertex[0] - x) * r
        y += (vertex[1] - y) * r
        points.append((x, y))
        colors.append(color)
    
    xs, ys = zip(*points)
    sc.set_offsets(points)
    sc.set_color(colors)
    return sc,

ani = animation.FuncAnimation(fig, update, frames=300, interval=50, blit=True)
plt.close()  # Prevents extra static plot
from IPython.display import HTML
HTML(ani.to_jshtml())  # Renders animation inline
