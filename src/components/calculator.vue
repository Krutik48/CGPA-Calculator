<template>
    <h2>IIT Madras CGPA Calculator</h2>
    <hr>
    <div class="d-flex flex-column justify-content-center align-items-center">
        <table>
            <thead>
                <tr>
                    <th>Course No</th>
                    <th>Course Name</th>
                    <th>Course Credit</th>
                    <th>Course Grade</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(course, index) in courses" :key="course.name">
                    <td>{{ course.no }}</td>
                    <td>{{ course.name }}</td>
                    <td>{{ course.credit }}</td>
                    <td>
                        <span v-if="editing !== index">{{ course.grade }}</span>
                        <form v-if="editing === index" @submit.prevent="saveEdit(index)">
                            <select v-model="course.grade" style="display: block;" class="form-select">
                                <option value="S">S</option>
                                <option value="A">A</option>
                                <option value="B">B</option>
                                <option value="C">C</option>
                                <option value="D">D</option>
                                <option value="E">E</option>
                                <option value="F">U</option>
                            </select>
                            <button class="btn btn-outline-dark editBtn" type="submit">
                                <svg xmlns="http://www.w3.org/2000/svg" width="19" height="19" fill="currentColor"
                                    class="bi bi-check-lg" viewBox="0 0 16 16">
                                    <path
                                        d="M12.736 3.97a.733.733 0 0 1 1.047 0c.286.289.29.756.01 1.05L7.88 12.01a.733.733 0 0 1-1.065.02L3.217 8.384a.757.757 0 0 1 0-1.06.733.733 0 0 1 1.047 0l3.052 3.093 5.4-6.425a.247.247 0 0 1 .02-.022Z" />
                                </svg>
                            </button>
                            <button class="btn btn-outline-danger editBtn" @click="cancelEdit">
                                <svg xmlns="http://www.w3.org/2000/svg" width="19" height="19" fill="currentColor"
                                    class="bi bi-x" viewBox="0 0 16 16">
                                    <path
                                        d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z" />
                                </svg>
                            </button>
                        </form>
                    </td>
                    <td>
                        <button class="btn btn-outline-danger" @click="deleteCourse(index)">
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                class="bi bi-trash3 icn" viewBox="0 0 16 16">
                                <path
                                    d="M6.5 1h3a.5.5 0 0 1 .5.5v1H6v-1a.5.5 0 0 1 .5-.5ZM11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3A1.5 1.5 0 0 0 5 1.5v1H2.506a.58.58 0 0 0-.01 0H1.5a.5.5 0 0 0 0 1h.538l.853 10.66A2 2 0 0 0 4.885 16h6.23a2 2 0 0 0 1.994-1.84l.853-10.66h.538a.5.5 0 0 0 0-1h-.995a.59.59 0 0 0-.01 0H11Zm1.958 1-.846 10.58a1 1 0 0 1-.997.92h-6.23a1 1 0 0 1-.997-.92L3.042 3.5h9.916Zm-7.487 1a.5.5 0 0 1 .528.47l.5 8.5a.5.5 0 0 1-.998.06L5 5.03a.5.5 0 0 1 .47-.53Zm5.058 0a.5.5 0 0 1 .47.53l-.5 8.5a.5.5 0 1 1-.998-.06l.5-8.5a.5.5 0 0 1 .528-.47ZM8 4.5a.5.5 0 0 1 .5.5v8.5a.5.5 0 0 1-1 0V5a.5.5 0 0 1 .5-.5Z" />
                            </svg>
                        </button>
                        <button class="btn btn-outline-dark mx-1" @click="startEdit(index)" v-if="editing !== index">
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                                class="bi bi-pencil-square icn" viewBox="0 0 16 16">
                                <path
                                    d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z" />
                                <path fill-rule="evenodd"
                                    d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z" />
                            </svg>
                        </button>
                    </td>
                </tr>
                <tr>
                    <td>
                        <input type="text" v-model="course.no" class="form-control" placeholder="Course No"
                            list="courseNolist" v-on:input="onAddCourseNo" />
                        <datalist id="courseNolist">
                            <option v-for="(course, index) in courseData" :key="index" :value="course.course_no"
                                :id="index" />
                        </datalist>
                    </td>
                    <td>
                        <input type="text" v-model="course.name" class="form-control" placeholder="Course Name"
                            list="courseNameList" v-on:input="onClickCourseName" />
                        <datalist id="courseNameList">
                            <option v-for="(course, index) in courseData" :key="index" :value="course.course" :id="index" />
                        </datalist>
                    </td>
                    <td class="grdCred">
                        <input type="number" v-model.number="course.credit" class="form-control" />
                    </td>
                    <td class="grdCred">
                        <select v-model="course.grade" style="display: block;" class="form-select">
                            <option value="S">S</option>
                            <option value="A">A</option>
                            <option value="B">B</option>
                            <option value="C">C</option>
                            <option value="D">D</option>
                            <option value="E">E</option>
                        </select>
                    </td>
                    <td>
                        <button type="submit" class="btn btn-dark my-2" @click="addCourse" style="display: table-cell;">Add
                            Course</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <h3>CGPA: {{ cgpa.toFixed(3) }}</h3>
    </div>
</template>

<script>

import courseData from "../data";

export default {
    name: 'cgpaCalculator',
    data() {
        return {
            course: {
                no: "",
                name: "",
                credit: 10,
                grade: "A"
            },
            courses: [],
            cgpa: 0,
            courseData: courseData,
            editing: null
        };
    },
    created() {
        document.title = "CGPA Calculator"
        this.courses = JSON.parse(localStorage.getItem("courses")) || [];
        this.calculateCGPA();
    },
    mounted() {
        document.addEventListener('keydown', this.handleKeyDown);
    },
    methods: {
        onClickCourseName() {
            let len = this.courseData.length
            for (let i = 0; i < len; i++) {
                if (this.courseData[i].course == this.course.name) {
                    this.course.no = this.courseData[i].course_no;
                    this.course.credit = this.courseData[i].credit;
                }
            }
        },
        startEdit(index) {
            this.course = this.courses[index];
            this.editing = index;
        },
        saveEdit(index) {
            console.log(this.course)
            this.courses.splice(index, 1, this.course);
            this.editing = null;
            this.calculateCGPA();
            this.resetCourse();
            this.saveCourses();
        },
        cancelEdit() {
            this.editing = null;
            this.resetCourse();
        },
        onAddCourseNo() {
            if (this.course.no.length == 6 || this.course.length == 7) {
                let len = this.courseData.length
                for (let i = 0; i < len; i++) {
                    if (this.courseData[i].course_no == this.course.no) {
                        this.course.name = this.courseData[i].course;
                        this.course.credit = this.courseData[i].credit;
                    }
                }
            }
        },
        addCourse() {
            this.courses.push(this.course);
            this.calculateCGPA();
            this.resetCourse();
            this.saveCourses();
        },
        calculateCGPA() {
            if (this.courses.length == 0) {
                this.cgpa = 0;
                return;
            }
            let totalCredits = 0;
            let gainCredit = 0;
            this.courses.forEach(course => {
                let grade = course.grade;
                let credit = course.credit;
                switch (grade.toLowerCase()) {
                    case 's':
                        gainCredit += credit;
                        break;
                    case 'a':
                        gainCredit += (credit) * 0.9;
                        break;
                    case 'b':
                        gainCredit += (credit) * 0.8;
                        break;
                    case 'c':
                        gainCredit += credit * 0.7;
                        break;
                    case 'd':
                        gainCredit += credit * 0.6;
                        break;
                    case 'e':
                        gainCredit += credit * 0.4;
                        break;
                    case 'u':
                        gainCredit += credit * 0;
                        break;
                    default:
                        break;
                }
                totalCredits += credit;
            });
            this.cgpa = gainCredit / totalCredits * 10;
        },
        resetCourse() {
            this.course = {
                no: "",
                name: "",
                credit: 10,
                grade: "A"
            };
        },
        deleteCourse(index) {
            this.courses.splice(index, 1);
            this.calculateCGPA();
            this.saveCourses();
        },
        saveCourses() {
            localStorage.setItem("courses", JSON.stringify(this.courses));
        },
        handleKeyDown(e) {
            if (e.key === 'Enter') {
                if (this.editing !== null) {
                    this.saveEdit(this.editing);
                } else {
                    this.addCourse();
                }
            }
        }
    },

}
</script>

<style>
@import url(https://fonts.googleapis.com/css?family=Roboto:400,500,700,300,100);

* {
    font-family: "Roboto";
}

table {
    margin-left: 10px;
    margin-right: 10px;
    margin-bottom: 10px;
    text-rendering: optimizeLegibility;
    font-weight: 400;
    text-align: center;
    border-collapse: collapse;
    box-shadow: 0px 2px 10px gray;
    border-radius: 7px;
}

th {
    color: #D5DDE5;
    ;
    background: #1b1e24;
    border-bottom: 4px solid #9ea7af;
    border-right: 1px solid #343a45;
    text-shadow: 0 1px 1px rgba(0, 0, 0, 0.1);
    font-size: 19px;
    font-weight: 300;
    padding: 0.8em
}

th:first-child {
    border-top-left-radius: 7px;
}

th:last-child {
    border-top-right-radius: 7px;
    border-right: none;
}

tr {
    border-top: 1px solid #C1C3D1;
    border-bottom: 1px solid #C1C3D1;
    background: #f6f6f6;
    color: #666B85;
}

tr:first-child {
    border-top: none;
}

tr:last-child {
    border-bottom: none;
}

tr:last-child td:first-child {
    border-bottom-left-radius: 7px;
}

tr:last-child td:last-child {
    border-bottom-right-radius: 7px;
}

td {
    padding: .5em;
    border-right: 1px solid #C1C3D1;
    font-weight: 300;
    text-shadow: -1px -1px 1px rgba(0, 0, 0, 0.1);
    font-size: 18px;
}

td:last-child {
    border-right: 0px;
}

.grdCred {
    width: 150px;
}

.icn {
    height: 15px;
    width: 15px;
}

.editBtn {
    margin-top: 6px;
    margin-left: 3px;
    margin-right: 3px;
}</style>