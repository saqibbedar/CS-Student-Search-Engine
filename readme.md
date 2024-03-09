const dataContainer = document.querySelector("[data-container]");
let input = document.getElementById("input");

let students = [];

input.addEventListener('input', (e) => {
    const inputValue = e.target.value.toLowerCase();
    students.forEach((student) => {
        const isVisible = student.Id.toLowerCase().includes(inputValue) || student.name.toLowerCase().includes(inputValue);
        student.Element.classList.toggle("hide", !isVisible);
    });
});

const fetchData = async () => {
    try {
        const response = await fetch("data.json");
        if (!response.ok) {
            throw new Error("Could not locate the resources.");
        }
        const data = await response.json();

        students = data.map((studentData) => {
            const cloneStudent = document.querySelector(".students").cloneNode(true);
            cloneStudent.classList.remove("hide");
            const header = cloneStudent.querySelector(".id");
            const body = cloneStudent.querySelector(".name");

            header.textContent = studentData.Id;
            body.textContent = studentData.StudentName; // Corrected from studentData.name

            dataContainer.append(cloneStudent);

            return { Id: studentData.Id, name: studentData.StudentName, Element: cloneStudent };
        });
    } catch (error) {
        console.error(error);
    }
}

fetchData();



let button = document.querySelector(".btn");

button.addEventListener("click", (e)=>{
    e.stopPropagation();
    input.classList.add("input-active");
})

dataContainer.addEventListener("click", ()=>{
    input.classList.remove("input-active")
})

