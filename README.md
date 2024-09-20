
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ProjectC_
{
    internal class Student
    {
        private int id;
        private string name;
        private float averageScore;
        private string faculty;
         
        public Student() { }
        public Student(int id, string name, float averageScore, string faculty)
        {
            this.Id = id;
            this.Name = name;
            this.AverageScore = averageScore;
            this.Faculty = faculty;
        }
        //ham nhap
        public void input( ) {
            Console.WriteLine("nhap MSSV: ");
            id = int.Parse(Console.ReadLine());
            Console.WriteLine("nhap ho ten: ");
            name = Console.ReadLine();
            Console.WriteLine("nhap diem trung binh: ");
            averageScore = float.Parse(Console.ReadLine());
            Console.WriteLine("nhap Khoa: ");
            faculty = Console.ReadLine();
        }
        public void show()
        {
            Console.WriteLine("MSSV: {0} || ho ten: :{1} || DTB: {2} || Khoa: {3} ",this.id,this.name,this.averageScore,this.faculty);
            }

        internal void dis()
        {
            throw new NotImplementedException();
        }

        internal void Display()
        {
            throw new NotImplementedException();
        }

        public int Id { get => id; set => id = value; }
        public string Name { get => name; set => name = value; }
        public float AverageScore { get => averageScore; set => averageScore = value; }
        public string Faculty { get => faculty; set => faculty = value; }
    }
}



//---------------------------------
using System;
using System.Collections.Generic;
using System.Linq;

namespace ProjectC_
{
    internal class Program
    {
        static void Main(string[] args)
        {
            List<Student> students = new List<Student>();
            bool exit = false;

            while (!exit)
            {
                Console.WriteLine("=====MENU=====");
                Console.WriteLine("1. Them 1 sinh vien ");
                Console.WriteLine("2. Danh sach sinh vien ");
                Console.WriteLine("3. 3 sinh vien co diem cao nhat");
                Console.WriteLine("4. sinh vien thuoc khoa CNTT ");
                Console.WriteLine("5. Sinh vien DTB tren 5");
                Console.WriteLine("6. DTB sap xep tang dan");
                Console.WriteLine("7. Danh sach sinh vien co diem TB > 5 thuoc khoa CNTT");
                Console.WriteLine("8. Sinh vien co DTB cao nhat thuoc khoa CNTT");
                Console.WriteLine("9. Phan loai thanh tich hoc sinh ");
                Console.WriteLine("0. Thoat");

                // Nhap lua chon
                string choice = Console.ReadLine();

                switch (choice)
                {
                    case "1":
                        AddStudent(students);  // Call static method to add a student
                        break;
                    case "2":
                        DisplayStudents(students);  // Call static method to display students
                        break;
                    case "3":
                        DisplayTop3Students(students);  // Display top 3 students with highest scores
                        break;
                    case "4":

                        DisplayStudentsofFaculty(students, "CNTT");
                        break;
                    case "5":
                        DisplayStudentsAVG(students, 5);
                        break;
                    case "6":
                        DisplayStudentsAVGsapxep(students);
                        break;
                    case "7":
                        DisplayStudentsFacultyandScore(students, "CNTT", 5.0f);
                        break;
                    case "8":
                        DisplayStudentsMAXDTBofCNTT(students, "CNTT");
                        break;
                    case "9":
                        Phanloai(students);
                        break;
                    case "0":
                        exit = true;
                        Console.WriteLine("Ket thuc chuong trinh");
                        break;
                    default:
                        Console.WriteLine("Lua chon khong hop le");
                        break;
                }
            }
        }

        // Static method to add a student
        public static void AddStudent(List<Student> lstStudent)
        {
            Student st = new Student();
            st.input();  // Call Input method to get student details
            lstStudent.Add(st);
            Console.WriteLine("Them sinh vien thanh cong!");
        }

        // Static method to display students
        public static void DisplayStudents(List<Student> lstStudent)
        {
            if (lstStudent.Count == 0)
            {
                Console.WriteLine("Danh sach sinh vien rong.");
                return;
            }

            Console.WriteLine("Danh sach sinh vien:");
            foreach (Student student in lstStudent)
            {
                student.show();  // Call Display method for each student
            }
        }
        public static void DisplayTop3Students(List<Student> lstStudent)
        {
            if (lstStudent.Count < 3)
            {
                Console.WriteLine("Khong co du sinh vien de hien thi top 3.");
                return;
            }

            // Sort the students by their scores in descending order and take the top 3
            var top3Students = lstStudent.OrderByDescending(s => s.AverageScore).Take(3).ToList();

            Console.WriteLine("Top 3 sinh vien co diem cao nhat:");
            foreach (var student in top3Students)
            {
                student.show();
            }
        }
        public static void DisplayStudentsofFaculty(List<Student> lstStudent, string faculty)
        {
            Console.WriteLine("===Danh sach sinh vien thuoc khoa {0}:", faculty);
            var studentsFaculty = lstStudent.Where(s => s.Faculty.Equals(faculty, StringComparison.OrdinalIgnoreCase));
            if (lstStudent.Count == 0)
            {
                Console.WriteLine("Khong co sinh vien nao thuoc khoa {0}.", faculty);
                return;
            }


            foreach (var student in studentsFaculty)
            {
                student.show();
            }// Display the filtered list of students
        }
        public static void DisplayStudentsAVG(List<Student> lstStudent, float MINDTB)
        {
            Console.WriteLine("=== Danh sach sinh vien co diem TB >= {0}", MINDTB);
            var Avg = lstStudent.Where(s => s.AverageScore >= MINDTB);

            foreach (var student in Avg)
            {
                student.show();
            }
        }
        public static void DisplayStudentsAVGsapxep(List<Student> lstStudent)
        {
            Console.WriteLine("=== Danh sach DTB tang dan");
            var studentsTangdan = lstStudent.OrderBy(s => s.AverageScore).ToList();

            foreach (var student in studentsTangdan)
            {
                student.show();
            }
        }
        public static void DisplayStudentsFacultyandScore(List<Student> lstStudent, string faculty, float MINDTB)
        {
            Console.WriteLine("=== Danh sach sinh vien co DTB >= {0} va thuoc khoa {1} ", MINDTB, faculty);
            var students = lstStudent.Where(s => s.AverageScore >= MINDTB && s.Faculty.Equals(faculty, StringComparison.OrdinalIgnoreCase)).ToList();
            foreach (var student in students)
            {
                student.show();
            }
        }
        public static void DisplayStudentsMAXDTBofCNTT(List<Student> lstStudent, string faculty)
        {
            // Lọc sinh viên theo khoa CNTT
            var studentsInFaculty = lstStudent.Where(s => s.Faculty.Equals(faculty, StringComparison.OrdinalIgnoreCase)).ToList();

            // Kiểm tra nếu không có sinh viên thuộc khoa CNTT
            if (studentsInFaculty.Count == 0)
            {
                Console.WriteLine("Khong co sinh vien nao thuoc khoa {0}.", faculty);
                return;
            }

            // Tìm điểm trung bình cao nhất
            float maxScore = studentsInFaculty.Max(s => s.AverageScore);

            // Lọc sinh viên có điểm trung bình cao nhất
            var topStudents = studentsInFaculty.Where(s => s.AverageScore == maxScore).ToList();

            // Hiển thị sinh viên có điểm cao nhất
            Console.WriteLine("=== Danh sach sinh vien co diem cao nhat thuoc khoa {0} ===", faculty);
            foreach (var student in topStudents)
            {
                student.show(); // Gọi hàm hiển thị thông tin sinh viên
            }
        }
        public static void Phanloai(List<Student> lstStudent)
        {

            if (lstStudent.Count == 0)
            {
                Console.WriteLine("Danh sach sinh vien rong.");
                return;
            }

            Console.WriteLine("=== Danh sach xep loai sinh vien ===");
            foreach (var student in lstStudent)
            {
                string grade;
                if (student.AverageScore >= 9.0 && student.AverageScore <= 10)
                {
                    grade = "Xuat sac";
                }
                else if (student.AverageScore >= 8.0 && student.AverageScore < 9.0)
                {
                    grade = "Gioi";
                }
                else if (student.AverageScore >= 7.0 && student.AverageScore < 8.0)
                {
                    grade = "Kha";
                }
                else if (student.AverageScore >= 5.0 && student.AverageScore < 7.0)
                {
                    grade = "Trung binh";
                }
                else if (student.AverageScore >= 4.0 && student.AverageScore < 5.0)
                {
                    grade = "Yeu";
                }
                else
                {
                    grade = "Kem";
                }

                // Hiển thị thông tin sinh viên cùng với xếp loại
                student.show();
                Console.WriteLine("Xep loai: " + grade);
            }
        }

    }

}






