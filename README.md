import java.util.*;
import java.util.stream.Collectors;
import java.util.function.Supplier;


public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);

        // 1. Read jobs from user
        System.out.print("Enter number of jobs: ");
        int n = in.nextInt();
        List<Job> jobs = new ArrayList<>();

        for (int i = 1; i <= n; i++) {
            System.out.printf("Job %d arrival time: ", i);
            int at = in.nextInt();
            System.out.printf("Job %d burst time:   ", i);
            int bt = in.nextInt();
            System.out.printf("Job %d priority:     ", i);
            int pr = in.nextInt();
            jobs.add(new Job(i, at, bt, pr));
        }

        // 2. Menu loop
        while (true) {
            System.out.println("\nChoose scheduling algorithm:");
            System.out.println(" 1. First-Come, First-Served (FCFS)");
            System.out.println(" 2. Shortest-Job-First (SJF) (non-preemptive)");
            System.out.println(" 3. Priority Scheduling (non-preemptive)");
            System.out.println(" 4. Round-Robin (RR)");
            System.out.println(" 5. Run all");
            System.out.println(" 0. Exit");
            System.out.print("Selection: ");
            int choice = in.nextInt();
            if (choice == 0) {
                System.out.println("Exiting.");
                break;
            }

            // Helper to copy original jobs into a fresh mutable list
            Supplier<List<Job>> copyJobs = () -> jobs.stream()
                .map(j -> new Job(j.getId(), j.getArrivalTime(), j.getBurstTime(), j.getPriority()))
                .collect(Collectors.toCollection(ArrayList::new));

            switch (choice) {
                case 1 -> {
                    List<Job> fcfsJobs = copyJobs.get();
                    Scheduler.printStats("FCFS", Scheduler.fcfs(fcfsJobs));
                }
                case 2 -> {
                    List<Job> sjfJobs = copyJobs.get();
                    Scheduler.printStats("SJF (non-preemptive)", Scheduler.sjf(sjfJobs));
                }
                case 3 -> {
                    List<Job> prioJobs = copyJobs.get();
                    Scheduler.printStats("Priority Scheduling", Scheduler.priorityScheduling(prioJobs));
                }
                case 4 -> {
                    System.out.print("Enter time quantum: ");
                    int q = in.nextInt();
                    List<Job> rrJobs = copyJobs.get();
                    Scheduler.printStats("Round-Robin (quantum=" + q + ")", Scheduler.roundRobin(rrJobs, q));
                }
                case 5 -> {
                    List<Job> a1 = copyJobs.get();
                    Scheduler.printStats("FCFS", Scheduler.fcfs(a1));
                    List<Job> a2 = copyJobs.get();
                    Scheduler.printStats("SJF (non-preemptive)", Scheduler.sjf(a2));
                    List<Job> a3 = copyJobs.get();
                    Scheduler.printStats("Priority Scheduling", Scheduler.priorityScheduling(a3));
                    System.out.print("Enter time quantum for RR: ");
                    int q2 = in.nextInt();
                    List<Job> a4 = copyJobs.get();
                    Scheduler.printStats("Round-Robin (quantum=" + q2 + ")", Scheduler.roundRobin(a4, q2));
                }
                default -> System.out.println("Invalid choice, try again.");
            }
        }

        in.close();
    }
}
