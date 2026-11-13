# Используем официальный ROS 2 Jazzy на Ubuntu 24.04 как базу; образ уже содержит ROS, CMake, Python, pytest, GoogleTest и colcon.
FROM ros:jazzy-ros-base-noble

# Устанавливаем MuJoCo для ros2_control, контроллеры, xacro и публикацию состояния робота.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ros-jazzy-mujoco-ros2-control \
        ros-jazzy-ros2-controllers \
        ros-jazzy-xacro \
        ros-jazzy-robot-state-publisher \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Создаём группу render с типичным для Ubuntu GID 109 для доступа обычного пользователя к графическому устройству.
RUN groupadd --gid 109 render

# Используем пользователя ubuntu с UID/GID 1000, убираем административную группу и готовим ему рабочую директорию.
RUN mkdir -p /workspace \
    && chown ubuntu:ubuntu /workspace \
    && gpasswd --delete ubuntu sudo

# Добавляем путь к ROS 2 в окружение всех процессов, включая команды, запущенные через docker compose exec.
ENV PATH="/opt/ros/jazzy/bin:${PATH}"

# Загружаем полное окружение ROS 2 при открытии интерактивной оболочки.
RUN echo 'source /opt/ros/jazzy/setup.bash' >> /home/ubuntu/.bashrc

# Назначаем рабочей директорией примонтированную папку проекта.
WORKDIR /workspace

# Запускаем команды контейнера от обычного пользователя, а не от root.
USER ubuntu

# Открываем интерактивную оболочку по умолчанию; Compose переопределяет команду для постоянной работы контейнера.
CMD ["bash"]
