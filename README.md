# IFC Bounding Box Calculator

Calculate bounding box dimensions and vertex coordinates for IFC objects, supporting accurate coordinate transformation for rotated elements.

## Table of Contents
- [Description](#description)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Code Structure](#code-structure)
- [Contribution](#contribution)
- [License](#license)

## Description
This Python tool processes IFC (Industry Foundation Classes) files to compute:
- Bounding box dimensions (length, width, height)
- Local and global coordinates for 8 bounding box vertices
- Correct coordinate transformation for objects with arbitrary rotations (fixes axis swapping issues in 90° rotations)

It uses `ifcopenshell` for IFC file parsing and geometric operations, with numpy for matrix transformations.

## Features
- 📐 Accurate bounding box calculation for IFC objects
- 🔄 Correct coordinate transformation for rotated elements (right-hand coordinate system compliance)
- 📏 Dimension calculation based on geometric local coordinates
- 🛠️ Error handling for file operations and object processing

## Installation
1. Install required packages:pip install ifcopenshell numpy
## Usage
### Example Codeif __name__ == "__main__":
    file_path = "/path/to/your_model.ifc"
    bboxes = get_objects_bounding_boxes(file_path, "IfcWall")
    
    for guid, data in bboxes.items():
        if "error" in data:
            print(f"Error processing {guid}: {data['error']}")
            continue
        
        print(f"\nObject Type: {data['type']} (GlobalID: {guid})")
        print(f"Dimensions (LxWxH): {data['dimensions']['L']} x {data['dimensions']['W']} x {data['dimensions']['H']}m")
        
        print("\nLocal Coordinates (Bounding Box Vertices):")
        for i, vertex in enumerate(data['coordinates']['local']):
            print(f"Vertex {i}: {vertex}")
        
        print("\nGlobal Coordinates (Bounding Box Vertices):")
        for i, vertex in enumerate(data['coordinates']['global']):
            print(f"Vertex {i}: {vertex}")
### Parameters
- `ifc_file_path`: Path to the IFC file
- `ifc_type`: Target IFC object type (e.g., "IfcWall", "IfcColumn", "IfcBeam")

## Code Structure
### Key Functions
1. **Coordinate Transformation**
   - `local_to_global(transform_matrix, local_point)`: Converts local coordinates to global using homogeneous transformation
   - `get_placement_matrix(placement)`: Builds transformation matrix from IFC local placement, handling hierarchical placements and rotations

2. **Bounding Box Calculation**
   - `get_bbox_vertices(bbox_min, bbox_max)`: Generates 8 vertices from bounding box min/max coordinates
   - `get_objects_bounding_boxes(ifc_file_path, ifc_type)`: Main entry point to process IFC file and return bounding box data for target object types

### Data Structure
Returns a dictionary with:{
    "type": object_type,
    "dimensions": {
        "L": length,
        "W": width,
        "H": height
    },
    "coordinates": {
        "local": list of local coordinates,
        "global": list of global coordinates
    }
}
## Contribution
1. Fork the repository
2. Create feature branch: `git checkout -b feature/new-function`
3. Commit changes: `git commit -m "Add new feature"`
4. Push to branch: `git push origin feature/new-function`
5. Create pull request

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
Developed for accurate geometric processing of parametric building models in AEC industry.
