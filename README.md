<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Maze Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Maze Game</h1>
    <p>Use the arrow keys to navigate through the maze and reach the exit!</p>
    
    <div class="game-container">
        <div class="maze" id="maze">
            <div class="player" id="player"></div>
            <div class="exit" id="exit"></div>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>

// Select the player and maze container
const player = document.getElementById("player");
const maze = document.getElementById("maze");
const exit = document.getElementById("exit");

// Set up the grid size (20x20 maze)
const gridSize = 20;
const cellSize = 100 / gridSize; // 100% / grid size for responsive cells

// Define the player's initial position in the grid (row and column)
let playerPosition = { x: 0, y: 0 };

// Define the exit position (bottom-right corner)
const exitPosition = { x: gridSize - 1, y: gridSize - 1 };

// Define walls in the maze (hardcoded for moderate difficulty)
const walls = [
    { x: 0, y: 1 }, { x: 2, y: 0 },{ x: 3, y: 0 },{ x: 4, y: 0 },  { x: 5, y: 1 }, { x: 5, y: 0 }, { x: 6, y: 1 }, { x: 7, y: 0 }, { x: 8, y: 1 }, 
    { x: 11, y: 1},  { x: 13, y: 1},    { x: 18, y: 1 }, { x: 19, y: 0 },{ x: 7, y: 4 },{ x: 16, y: 0 },{ x: 17, y: 0 },{ x: 11, y: 0 },{ x: 10, y: 0 },{ x: 14, y: 0 },{ x: 17, y: 1 },

    { x: 0, y: 2 }, { x: 1, y: 2 }, { x: 2, y: 2 }, { x: 3, y: 2 },   { x: 6, y: 2 }, { x: 7, y: 2 }, { x: 8, y: 2 }, 
    { x: 10, y: 2 },   { x: 15, y: 2 },   { x: 19, y: 2 },

    { x: 0, y: 3 }, { x: 1, y: 3 }, { x: 2, y: 3 },  { x: 4, y: 3 },  { x: 6, y: 3 }, { x: 7, y: 3 },    { x: 12, y: 3 }, { x: 13, y: 3 },  { x: 15, y: 3 },  { x: 17, y: 3 }, { x: 18, y: 3 }, { x: 19, y: 3 },

      { x: 4, y: 4 },  { x: 9, y: 4 },
    { x: 10, y: 4 },  { x: 13, y: 4 },  { x: 15, y: 4 },   { x: 18, y: 4 }, { x: 19, y: 4 },

     { x: 1, y: 5 }, { x: 2, y: 5 },  { x: 4, y: 5 },  
    { x: 10, y: 5 },  { x: 12, y: 5 }, { x: 13, y: 5 }, { x: 15, y: 5 }, { x: 16, y: 5 },  { x: 18, y: 5 }, 

     { x: 1, y: 6 },   { x: 4, y: 6 }, { x: 5, y: 6 }, { x: 6, y: 6 }, { x: 7, y: 6 }, { x: 8, y: 6 }, 
    { x: 10, y: 6 },  { x: 12, y: 6 }, { x: 15, y: 6 }, { x: 16, y: 6 },  { x: 19, y: 6 },

     { x: 1, y: 7 }, { x: 2, y: 7 }, 
    { x: 10, y: 7 }, { x: 12, y: 7 }, { x: 13, y: 7 }, { x: 15, y: 7 }, { x: 16, y: 7 },  { x: 18, y: 7 }, 

     { x: 1, y: 8 }, { x: 2, y: 8 }, { x: 3, y: 8 }, { x: 4, y: 8 }, { x: 5, y: 8 }, { x: 6, y: 8 }, { x: 7, y: 8 }, { x: 8, y: 8 }, { x: 9, y: 8 },
    { x: 10, y: 8 }, { x: 12, y: 8 },   { x: 15, y: 8 },  { x: 18, y: 8 }, 

       { x: 12, y: 9 },  { x: 15, y: 9 }, { x: 16, y: 9 },, { x: 2, y: 10 }, { x: 3, y: 10 }, { x: 4, y: 10 }, { x: 5, y: 10 }, { x: 6, y: 10 }, { x: 7, y: 10 }, { x: 8, y: 10 }, { x: 9, y: 10 },
    { x: 10, y: 10 }, { x: 11, y: 10 }, { x: 12, y: 10 },  { x: 15, y: 10 }, { x: 16, y: 10 }, { x: 17, y: 10 }, { x: 18, y: 10 }, { x: 19, y: 10 },

    { x: 0, y: 11 }, { x: 1, y: 11 }, { x: 2, y: 11 },   { x: 5, y: 11 }, 
        { x: 18, y: 11 }, { x: 19, y: 11 },

      { x: 5, y: 12 },  { x: 7, y: 12 }, { x: 8, y: 12 }, { x: 9, y: 12 },
    { x: 10, y: 12 }, { x: 11, y: 12 },  { x: 13, y: 12 }, { x: 14, y: 12 }, { x: 16, y: 12 }, { x: 17, y: 12 }, { x: 18, y: 12 }, { x: 19, y: 12 },

    { x: 1, y: 13 }, { x: 2, y: 13 }, { x: 3, y: 13 },  { x: 5, y: 13 },  { x: 7, y: 13 }, { x: 8, y: 13 }, 
    { x: 10, y: 13 }, { x: 11, y: 13 }, { x: 12, y: 13 }, { x: 13, y: 13 }, { x: 14, y: 13 }, { x: 16, y: 13 },

     { x: 1, y: 14 }, { x: 2, y: 14 }, { x: 3, y: 14 },    { x: 19, y: 14 },

     { x: 1, y: 15 },   { x: 5, y: 15 }, { x: 6, y: 15 }, { x: 7, y: 15 }, { x: 8, y: 15 }, { x: 9, y: 15 },
    { x: 10, y: 15 },  { x: 12, y: 15 }, { x: 13, y: 15 }, { x: 14, y: 15 }, { x: 15, y: 15 },  { x: 17, y: 15 }, { x: 18, y: 15 }, { x: 19, y: 15 },

    { x: 1, y: 16 },  { x: 3, y: 16 }, { x: 4, y: 16 },  { x: 6, y: 16 }, { x: 7, y: 16 },  { x: 9, y: 16 },
    { x: 10, y: 16 }, { x: 11, y: 16 }, { x: 14, y: 16 },   { x: 17, y: 16 }, { x: 18, y: 16 }, 

     { x: 1, y: 17 },    { x: 16, y: 17 }, { x: 17, y: 17 }, { x: 18, y: 17 },

     { x: 1, y: 18 }, { x: 2, y: 18 }, { x: 3, y: 18 }, { x: 4, y: 18 }, { x: 5, y: 18 }, { x: 6, y: 18 }, { x: 7, y: 18 }, { x: 8, y: 18 }, { x: 9, y: 18 },
    { x: 10, y: 18 }, { x: 11, y: 18 }, { x: 12, y: 18 }, { x: 13, y: 18 }, { x: 14, y: 18 }, { x: 15, y: 18 },  { x: 17, y: 18 }, { x: 18, y: 18 }, 
];


// Function to update the player's position
function movePlayer(x, y) {
    // Check if the new position is a wall
    if (isWall(x, y)) {
        return;
    }

    // Update the player's position
    playerPosition.x = x;
    playerPosition.y = y;

    // Update the player's grid position in CSS
    player.style.gridColumn = playerPosition.x + 1;
    player.style.gridRow = playerPosition.y + 1;

    // Check if the player has reached the exit
    if (x === exitPosition.x && y === exitPosition.y) {
        alert("Congratulations! You won the game!");
    }
}

// Function to check if a position is a wall
function isWall(x, y) {
    return walls.some(wall => wall.x === x && wall.y === y);
}

// Function to handle key presses for player movement
window.addEventListener("keydown", (e) => {
    switch (e.key) {
        case "ArrowUp":
            if (playerPosition.y > 0) {
                movePlayer(playerPosition.x, playerPosition.y - 1);
            }
            break;
        case "ArrowDown":
            if (playerPosition.y < gridSize - 1) {
                movePlayer(playerPosition.x, playerPosition.y + 1);
            }
            break;
        case "ArrowLeft":
            if (playerPosition.x > 0) {
                movePlayer(playerPosition.x - 1, playerPosition.y);
            }
            break;
        case "ArrowRight":
            if (playerPosition.x < gridSize - 1) {
                movePlayer(playerPosition.x + 1, playerPosition.y);
            }
            break;
    }
});

// Function to render the walls on the maze
function renderWalls() {
    walls.forEach(wall => {
        const wallElement = document.createElement("div");
        wallElement.classList.add("wall");
        wallElement.style.gridColumn = wall.x + 1;
        wallElement.style.gridRow = wall.y + 1;
        maze.appendChild(wallElement);
    });
}

// Initializing the game
renderWalls();
movePlayer(0, 0); // Set the player at the starting position

/* General page styling */
body {
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
    text-align: center;
}

h1 {
    font-size: 3rem;
    margin-top: 20px;
}

p {
    font-size: 1.2rem;
}

/* Game container styling */
.game-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #1e1e1e;
}

/* Maze container styling (fills the screen) */
.maze {
    position: relative;
    width: 80vmin;  /* Maze takes up 80% of the smallest screen dimension */
    height: 80vmin;
    background-color: #fff;
    border: 2px solid #000;
    display: grid;
    grid-template-columns: repeat(20, 1fr); /* Larger grid size */
    grid-template-rows: repeat(20, 1fr); /* Larger grid size */
    gap: 2px;
}

/* Styling for player */
.player {
    width: 100%;
    height: 100%;
    background-color: #ffcc00;
    border-radius: 50%;
    grid-column: 1;
    grid-row: 1;
    transition: all 0.2s ease-in-out;
}

/* Exit point styling */
.exit {
    width: 100%;
    height: 100%;
    background-color: green;
    grid-column: 20;
    grid-row: 20;
    border-radius: 50%;
    transition: all 0.2s ease-in-out;
}

/* Wall styling */
.wall {
    background-color: #000;
    transition: background-color 0.3s ease;
}
public class Student {
    private int id;
    private String name;
    private int age;

    public Student(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

import java.util.ArrayList;
import java.util.List;

public class StudentService {
    private List<Student> students = new ArrayList<>();

    public List<Student> getAllStudents() {
        return students;
    }

    public void addStudent(Student student) {
        students.add(student);
    }

    public void deleteStudent(int id) {
        students.removeIf(student -> student.getId() == id);
    }

    public Student getStudentById(int id) {
        for (Student student : students) {
            if (student.getId() == id) {
                return student;
            }
        }
        return null;
    }
}
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/students")
public class StudentServlet extends HttpServlet {
    private StudentService studentService = new StudentService();

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String action = request.getParameter("action");
        if (action == null) action = "list";

        switch (action) {
            case "list":
                request.setAttribute("students", studentService.getAllStudents());
                request.getRequestDispatcher("index.jsp").forward(request, response);
                break;
            case "add":
                request.getRequestDispatcher("add-student.jsp").forward(request, response);
                break;
            case "delete":
                int id = Integer.parseInt(request.getParameter("id"));
                studentService.deleteStudent(id);
                response.sendRedirect("students");
                break;
            default:
                response.getWriter().println("Invalid action");
        }
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String name = request.getParameter("name");
        int age = Integer.parseInt(request.getParameter("age"));
        int id = studentService.getAllStudents().size() + 1;

        Student student = new Student(id, name, age);
        studentService.addStudent(student);

        response.sendRedirect("students");
    }
}
<!DOCTYPE html>
<html>
<head>
    <title>Student Management</title>
</head>
<body>
    <h1>Student List</h1>
    <a href="students?action=add">Add New Student</a>
    <table border="1">
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Age</th>
            <th>Actions</th>
        </tr>
        <c:forEach var="student" items="${students}">
            <tr>
                <td>${student.id}</td>
                <td>${student.name}</td>
                <td>${student.age}</td>
                <td>
                    <a href="students?action=delete&id=${student.id}">Delete</a>
                </td>
            </tr>
        </c:forEach>
    </table>
</body>
</html>

<!DOCTYPE html>
<html>
<head>
    <title>Add Student</title>
</head>
<body>
    <h1>Add Student</h1>
    <form action="students" method="post">
        <label for="name">Name:</label>
        <input type="text" name="name" required><br>
        <label for="age">Age:</label>
        <input type="number" name="age" required><br>
        <button type="submit">Add</button>
    </form>
</body>
</html>
import java.util.ArrayList;
import java.util.List;

public class StudentService {
    private List<Student> students = new ArrayList<>();

    public List<Student> getAllStudents() {
        return students;
    }

    public void addStudent(Student student) {
        students.add(student);
    }

    public void deleteStudent(int id) {
        students.removeIf(student -> student.getId() == id);
    }

    public Student getStudentById(int id) {
        for (Student student : students) {
            if (student.getId() == id) {
                return student;
            }
        }
        return null;
    }

    public void updateStudent(int id, String name, int age) {
        Student student = getStudentById(id);
        if (student != null) {
            student.setName(name);
            student.setAge(age);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        StudentService service = new StudentService();

        // Add Students
        service.addStudent(new Student(1, "Alice", 20));
        service.addStudent(new Student(2, "Bob", 22));
        service.addStudent(new Student(3, "Charlie", 21));

        // List Students
        System.out.println("Students List:");
        for (Student student : service.getAllStudents()) {
            System.out.println(student);
        }

        // Update Student
        service.updateStudent(2, "Bob Updated", 23);
        System.out.println("\nAfter Update:");
        for (Student student : service.getAllStudents()) {
            System.out.println(student);
        }

        // Delete Student
        service.deleteStudent(1);
        System.out.println("\nAfter Deletion:");
        for (Student student : service.getAllStudents()) {
            System.out.println(student);
        }
    }
}
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet("/students")
public class StudentServlet extends HttpServlet {
    private StudentService studentService = new StudentService();

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String action = request.getParameter("action");
        if (action == null) action = "list";

        switch (action) {
            case "list":
                response.getWriter().println("Student List:");
                for (Student student : studentService.getAllStudents()) {
                    response.getWriter().println(student);
                }
                break;
            case "add":
                response.getWriter().println("To add a student, send a POST request.");
                break;
            case "delete":
                int id = Integer.parseInt(request.getParameter("id"));
                studentService.deleteStudent(id);
                response.getWriter().println("Student deleted.");
                break;
            case "update":
                response.getWriter().println("To update a student, send a POST request with 'update' action.");
                break;
            default:
                response.getWriter().println("Invalid action.");
        }
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String action = request.getParameter("action");
        if ("add".equals(action)) {
            String name = request.getParameter("name");
            int age = Integer.parseInt(request.getParameter("age"));
            int id = studentService.getAllStudents().size() + 1;

            Student student = new Student(id, name, age);
            studentService.addStudent(student);

            response.getWriter().println("Student added: " + student);
        } else if ("update".equals(action)) {
            int id = Integer.parseInt(request.getParameter("id"));
            String name = request.getParameter("name");
            int age = Integer.parseInt(request.getParameter("age"));

            studentService.updateStudent(id, name, age);
            response.getWriter().println("Student updated: " + studentService.getStudentById(id));
        } else {
            response.getWriter().println("Invalid POST action.");
        }
    }
}
