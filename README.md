using System;
using System.Collections.Generic;

class Program
{
    static void Main(string[] args)
    {
        Console.OutputEncoding = System.Text.Encoding.UTF8; // Set output encoding to UTF-8

        List<HealthRecord> records = new List<HealthRecord>();

        while (true)
        {
            Console.WriteLine("Health Tracking Application");
            Console.WriteLine("1. Record New Data");
            Console.WriteLine("2. View Health History");
            Console.WriteLine("3. Exit");
            Console.WriteLine("4. View Statistics"); // New option
            Console.Write("Choose an operation: ");

            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    RecordHealthData(records);
                    break;
                case "2":
                    ViewHealthHistory(records);
                    break;
                case "3":
                    Console.WriteLine("Thank you for using the Health Tracking Application.");
                    return;
                case "4":
                    DisplayStatistics(records);
                    break;
                default:
                    Console.WriteLine("Invalid option. Please choose again.");
                    break;
            }
        }
    }

    static void RecordHealthData(List<HealthRecord> records)
    {
        Console.Write("Enter name: ");
        string name = Console.ReadLine();

        Console.Write("Enter date (YYYY-MM-DD): ");
        string date = Console.ReadLine();
        DateTime recordDate;

        if (DateTime.TryParse(date, out recordDate))
        {
            Console.Write("Enter weight (kg): ");
            double weight = double.Parse(Console.ReadLine());

            Console.Write("Enter exercise duration (minutes): ");
            int exerciseDuration = int.Parse(Console.ReadLine());

            Console.Write("Enter sleep hours (hours): ");
            double sleepHours = double.Parse(Console.ReadLine());

            Console.Write("Enter calorie intake (kcal): ");
            int calories = int.Parse(Console.ReadLine());

            Console.Write("Enter gender (Male/Female): ");
            string gender = Console.ReadLine();

            HealthRecord record = new HealthRecord(name, recordDate, weight, exerciseDuration, sleepHours, calories, gender);
            records.Add(record);
            Console.WriteLine("Data has been recorded.");
        }
        else
        {
            Console.WriteLine("Invalid date format.");
        }
    }

    static void ViewHealthHistory(List<HealthRecord> records)
    {
        if (records.Count == 0)
        {
            Console.WriteLine("No historical data available.");
            return;
        }

        Console.WriteLine("Health History Records:");

        foreach (var record in records)
        {
            Console.WriteLine(record);
        }
    }

    static void DisplayStatistics(List<HealthRecord> records)
    {
        if (records.Count == 0)
        {
            Console.WriteLine("No historical data available.");
            return;
        }

        double totalWeight = 0;
        int totalExerciseDuration = 0;
        double totalSleepHours = 0;

        foreach (var record in records)
        {
            totalWeight += record.Weight;
            totalExerciseDuration += record.ExerciseDuration;
            totalSleepHours += record.SleepHours;
        }

        double averageWeight = totalWeight / records.Count;
        double averageExerciseDuration = (double)totalExerciseDuration / records.Count;
        double averageSleepHours = totalSleepHours / records.Count;

        Console.WriteLine("Health Data Statistics:");
        Console.WriteLine($"Average Weight: {averageWeight:F2} kg");
        Console.WriteLine($"Average Exercise Duration: {averageExerciseDuration:F2} minutes");
        Console.WriteLine($"Average Sleep Hours: {averageSleepHours:F2} hours");
    }

    // New function to output BMI and BMI range in English
    static void OutputBMIInfo(double bmi)
    {
        string bmiRange;

        if (bmi < 18.5)
        {
            bmiRange = "Underweight (BMI < 18.5)";
        }
        else if (bmi < 24)
        {
            bmiRange = "Normal Range (18.5 ≤ BMI < 24)";
        }
        else if (bmi < 27)
        {
            bmiRange = "Overweight (24 ≤ BMI < 27)";
        }
        else if (bmi < 30)
        {
            bmiRange = "Mildly Obese (27 ≤ BMI < 30)";
        }
        else if (bmi < 35)
        {
            bmiRange = "Moderately Obese (30 ≤ BMI < 35)";
        }
        else
        {
            bmiRange = "Severely Obese (BMI ≥ 35)";
        }

        Console.WriteLine("BMI: " + bmi);
        Console.WriteLine("BMI Range: " + bmiRange);
    }
}

class HealthRecord
{
    public string Name { get; }
    public DateTime Date { get; }
    public double Weight { get; }
    public int ExerciseDuration { get; }
    public double SleepHours { get; }
    public int CaloriesIntake { get; }
    public string Gender { get; }

    public HealthRecord(string name, DateTime date, double weight, int exerciseDuration, double sleepHours, int caloriesIntake, string gender)
    {
        Name = name;
        Date = date;
        Weight = weight;
        ExerciseDuration = exerciseDuration;
        SleepHours = sleepHours;
        CaloriesIntake = caloriesIntake;
        Gender = gender;
    }

    public double CalculateBMI()
    {
        double heightInMeters = 1.75; // Assuming height is 1.75 meters
        return Weight / (heightInMeters * heightInMeters);
    }

    public override string ToString()
    {
        double bmi = CalculateBMI();
        return $"Name: {Name}, Date: {Date.ToShortDateString()}, Weight: {Weight} kg, Exercise Duration: {ExerciseDuration} minutes, Sleep Hours: {SleepHours} hours, Calorie Intake: {CaloriesIntake} kcal, Gender: {Gender}, BMI: {bmi:F2}";
    }
}
