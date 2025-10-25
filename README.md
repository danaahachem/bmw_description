1. **Studied the original URDF**
   I started from the provided `BMW.urdf` file, which described the robot as one long XML document. It contained the chassis, wheels, sensors, inertial properties, and Gazebo plugins all in a single file.

2. **Identified logical sections**
   I separated the URDF into smaller logical components:

   * **Core** → chassis and four wheels
   * **Sensors** → lidar and camera
   * **Plugins** → Gazebo differential drive and sensor plugins

3. **Created modular XACRO files**
   I created one main file `bmw_robot.xacro`, which includes three subfiles inside a `parts` folder:

   ```
   urdf/
   ├── bmw_robot.xacro
   └── parts/
       ├── core.xacro
       ├── sensors.xacro
       └── plugins.xacro
   ```

   * Each part was converted into a **macro**, so it can be called cleanly from the main file.
   * The main file only includes those parts, defines robot name, and assembles the final model.

4. **Reconstructed the robot’s structure**
   I made sure each XACRO file recreated the same geometry, dimensions, and inertial values from the original URDF:

   * The **chassis** kept the 0.6 × 0.7 × 0.1 box geometry.
   * The **wheels** used cylinders (0.2 radius, 0.07 length) with proper inertial matrices.
   * The **lidar** and **camera** were positioned exactly at (-0.16, 0, 0.1) and (0.2, 0, 0.05), matching the original file.

5. **Handled the materials and joint connections**

   * Restored all original color definitions (`blue`, `gray`, `darkBlue`).
   * Preserved all joint types and axis directions.
   * Verified that the TF hierarchy matched the original link tree.

6. **Included Gazebo plugins separately**
   The diff-drive plugin, lidar sensor plugin, and camera plugin were moved into `plugins.xacro`.
   This makes it easier to enable or disable simulation functionality by simply commenting or uncommenting one line in the main file.

7. **Validated the model**

   * Used the `xacro` command to generate a clean `.urdf` file.
   * Verified it with `check_urdf` (no parsing or inertial errors).
   * Loaded it successfully in RViz 2 with all links visible and colored.
   * Confirmed wheel joint transforms using `robot_state_publisher` and `joint_state_publisher_gui`.

  <img width="421" height="501" alt="image" src="https://github.com/user-attachments/assets/91cc7f5b-5250-4c47-b75d-f5470779e7fe" />

  
