import pygame
import datetime

# Initialize Pygame
pygame.init()

# Set up the screen
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Islamic Player")  # Set the window title

# Set up fonts
font_prayer = pygame.font.Font(None, 36)   # Font for prayer name
font_instruction = pygame.font.Font(None, 20)

# List of predefined prayer times
#prayer_times = [
   # {'name': 'Dhuhr', 'time': datetime.time(13, 17)},
    #{'name': 'Asr', 'time': datetime.time(17, 23)},
    #{'name': 'Maghrib', 'time': datetime.time(20, 57)},
    #{'name': 'Isha', 'time': datetime.time(22, 45)}
#]

# Custom messages for each prayer
prayer_azan = {
    'Fajr': "Bismillah hir rahman nir raheem. In the Name of Allah, the Most Beneficent, the Most Merciful.",
    'Dhuhr': "As-salamu alaykum. Peace be upon you.",
    'Asr': "Alhamdulillah. All praise is due to Allah.",
    'Maghrib': "Subhanallah. Glory be to Allah.",
    'Isha': "Allahu Akbar. Allah is the Greatest."
}

# Sorting function for prayer times
def sort_prayer_times(prayer_times):
    return sorted(prayer_times, key=lambda x: x['time'])

# Function to get upcoming prayer time and name
def get_prayer_info():
    now = datetime.datetime.now()
    sorted_prayer_times = sort_prayer_times(prayer_times)

    # Find the upcoming prayer
    upcoming_prayer = None
    for prayer in sorted_prayer_times:
        prayer_time = datetime.datetime.combine(now.date(), prayer['time'])
        if prayer_time > now:
            upcoming_prayer = prayer
            break

    # If all prayers are done for the day, then set next prayer to tomorrow's Fajr
    if upcoming_prayer is None:
        tomorrow_fajr = {'name': 'Fajr', 'time': datetime.time(3, 50)}
        upcoming_prayer = tomorrow_fajr

    return upcoming_prayer

# Function to read prayer times from a file
def read_prayer_times_from_file(filename):
    prayer_times = []
    with open("prayers.txt", mode = 'r') as file:
        for line in file:
            name, hour, minute = line.strip().split(',')
            prayer_time = {'name': name, 'time': datetime.time(int(hour), int(minute))}
            prayer_times.append(prayer_time)
    return prayer_times

# Example usage
prayer_times = read_prayer_times_from_file('prayer_times.txt')


# Function to display text on screen
def draw_text(text, font, color, x, y):
    text_surface = font.render(text, True, color)
    screen.blit(text_surface, (x, y))

# Display the title in the middle of the screen
screen.fill((255, 255, 255))  # Clear the screen
draw_text("Islamic Player", font_prayer, (255, 0, 0), screen_width // 2, screen_height // 2)  # Centered title
pygame.display.flip()

# Main loop
running = True
show_prayer_info = False
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN:
            show_prayer_info = not show_prayer_info  # Toggle show_prayer_info

    screen.fill((255, 255, 255))

    # Display current time in Arabic and English
    current_time = datetime.datetime.now()
    arabic_time = current_time.strftime("%H:%M:%S")  # Format Arabic time
    english_time = current_time.strftime("%I:%M:%S %p")  # Format English time
    draw_text("Current Time (Arabic): " + arabic_time, font_prayer, (0, 0, 0), 50, 50)
    draw_text("Current Time (English): " + english_time, font_prayer, (0, 0, 0), 50, 100)

    # If any key is pressed, display upcoming prayer time and name
    if show_prayer_info:
        prayer_info = get_prayer_info()
        draw_text("Upcoming Prayer Time: " + prayer_info['time'].strftime("%I:%M %p"), font_prayer, (0, 0, 0), 50, 150)
        draw_text("Next Prayer: " + prayer_info['name'], font_prayer, (0, 0, 0), 50, 200)
        draw_text("Prayer Azan : " + prayer_azan[prayer_info['name']], font_prayer, (0, 0, 0), 50, 250)

    # Display instruction to press any key to toggle prayer info
    draw_text("Press any key to " + ("hide" if show_prayer_info else "show") + " prayer info", font_instruction, (100, 100, 100), 600, 550)

    pygame.display.flip()

# Quit Pygame
pygame.quit()


# List of predefined prayer information
prayer_info_list = [
    {'name': 'Fajr', 'time': datetime.time(3, 50), 'azan': "Bismillah hir rahman nir raheem. In the Name of Allah, the Most Beneficent, the Most Merciful."},
    {'name': 'Dhuhr', 'time': datetime.time(13, 17), 'azan': "As-salamu alaykum. Peace be upon you."},
    {'name': 'Asr', 'time': datetime.time(17, 23), 'azan': "Alhamdulillah. All praise is due to Allah."},
    {'name': 'Maghrib', 'time': datetime.time(20, 57), 'azan': "Subhanallah. Glory be to Allah."},
    {'name': 'Isha', 'time': datetime.time(22, 45), 'azan': "Allahu Akbar. Allah is the Greatest."}
]
