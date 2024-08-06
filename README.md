# Documentation: Modeling a Cone in Dassault Abaqus

## 1. Introduction
This document outlines the step-by-step process to model a cone using Dassault Abaqus.

## 2. Creating the Standard Explicit Model
1. **Start Abaqus/CAE**.
2. **Create a new model**:
    - Go to `File` > `New Model Database`.

## 3. Creating the Cone Part
1. **Create Part**:
    - Go to `Part` > `Create`.
    - Name: `Cone`.
    - Type: `Deformable`.
    - Base Feature: `Shell (Revolution)`.

2. **Sketch the Profile**:
    - Draw a slanted line on the right side of the axis.
    - Set the line length to `200 mm`.
    - Set the revolution angle to `90 degrees`.

## 4. Creating the Rigid Wall Part
1. **Create Part**:
    - Go to `Part` > `Create`.
    - Name: `Rigid_Wall`.
    - Type: `Analytical Rigid`.
    - Base Feature: `Extruded Shell`.

2. **Sketch the Profile**:
    - Draw a vertical line on the axis of `100 mm`.
    - Set the extrusion depth to `100 mm`.

3. **Create Reference Point**:
    - Go to `Tools` > `Datum` > `Point`.
    - Create a datum point at the middle of the extruded wall.
    - Go to `Tools` > `Reference Point` and select the datum point.

## 5. Defining Material Properties
1. **Create Material**:
    - Go to the `Property module` > `Material` > `Create`.
    - Name: `Material_1`.
    
2. **Set Density**:
    - Go to the `General` tab > `Density`.
    - Value: `2.7e-9`.

3. **Set Elastic Properties**:
    - Go to the `Mechanical` tab > `Elastic`.
    - Young's Modulus: `68,900`.
    - Poisson's Ratio: `0.33`.

4. **Set Plastic Properties**:
    - Go to the `Mechanical` tab > `Plastic`.
    - Yield Stress: `225` and `440.4`.
    - Plastic Strain: `0` and `0.12`.

## 6. Creating and Assigning Sections
1. **Create Section**:
    - Go to the `Property module` > `Section` > `Create`.
    - Category: `Homogeneous`.
    - Type: `Shell`.
    - Shell Thickness: `2 mm`.

2. **Assign Section to Cone**:
    - Go to the `Property module` > `Assign Section`.
    - Select the cone part and assign the previously created section.

3. **Define Inertia for Rigid Wall**:
    - Go to the `Property module` > `Special` > `Inertia Manager`.
    - Name: `Wall_Mass`.
    - Select the reference point datum.
    - Isotropic Mass: `0.075`.

## 7. Assembling the Parts
1. **Create Instances**:
    - Go to the `Assembly module` > `Instance` > `Create`.
    - Select both the cone and the wall parts.

2. **Position the Cone**:
    - Rotate the cone to hit the wall perpendicularly.
    - Set the X-vector start point to `1`.
    - Set the endpoint X vector to `10` with a `90°` rotation angle.

3. **Position the Wall**:
    - Rotate the wall using the top right and bottom right edge points with a `90°` rotation angle.
    - Translate the wall to position it near the cone with a `3.0 mm` separation on the Z-axis.

## 8. Defining Steps
1. **Create Step**:
    - Go to the `Step module` > `Create`.
    - Choose `Dynamic, Explicit`.
    - Set Time Period to `0.05`.

## 9. Defining Interaction Properties
1. **Create Surface**:
    - Go to the `Assembly module` > `Surfaces` > `Create`.
    - Select the rigid wall and shade it purple.

2. **Create Interaction Property**:
    - Go to the `Interaction module` > `Create Property`.
    - Name: `Friction`.
    - Type: `Contact`.
    - Tangential Behavior: `Penalty` with a friction coefficient of `0.2`.
    - Normal Behavior: Default settings.

3. **Create Interaction**:
    - Go to the `Interaction module` > `Create`.
    - Type: `General Contact` > `All with Self`.
    - Assign the friction property globally.

## 10. Applying Boundary Conditions
1. **Create Boundary Conditions**:
    - Go to the `Load module` > `Create Boundary Condition`.
    - Choose `Symmetry/Encastre`.
    - Select the cone base and apply `Encastre (U1=U2=...)`.

2. **Apply Symmetry Conditions**:
    - Apply `XSYMM (U1=UR2=0)` on one side of the cone parallel to the Z-axis.
    - Apply `YSYMM` on the other side.

3. **Apply Displacement/Rotation**:
    - Select the rigid wall's reference point.
    - Apply `U1, U2, UR1, UR2, UR3` as `0` from the CSYS global variable.

## 11. Defining Initial Conditions
1. **Create Initial Step**:
    - Go to the `Load module` > `Predefined Field` > `Create`.
    - Choose `Velocity`.
    - Select the reference point on the rigid wall.
    - Input `-7000` for `V3 (Z-axis velocity)`.

## 12. Meshing the Cone
1. **Mesh the Cone**:
    - Go to the `Mesh module` > `Create Mesh`.
    - Select the cone part.
    - Choose `Element Type` and set it to the `Explicit` element library.
    - Apply `Quad-dominated` mesh controls.
    - Set the global seed part and mesh the part.

## 13. Creating and Submitting the Job
1. **Create Job**:
    - Go to the `Job module` > `Create`.
    - Name the job and configure settings as required.
    - Submit the job for analysis.

---
