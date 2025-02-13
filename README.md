import tkinter as tk
from tkinter import messagebox

def calculate_swp():
    try:
        initial_amount = float(initial_amount_entry.get())
        withdrawal_amount = float(withdrawal_amount_entry.get())
        rate_of_return = float(rate_of_return_entry.get()) / 100
        frequency = frequency_var.get()
        duration = int(duration_entry.get())

        if frequency == "Yearly":
            duration *= 12

        remaining_amount = initial_amount
        result_text = ""

        for month in range(1, duration + 1):
            remaining_amount *= (1 + rate_of_return / 12)
            remaining_amount -= withdrawal_amount
            if remaining_amount < 0:
                result_text += f"Month {month}: Insufficient funds!\n"
                break
            result_text += f"Month {month}: Remaining Amount: ₹{remaining_amount:.2f}\n"

        result_label.config(text=result_text)
    except ValueError:
        messagebox.showerror("Error", "Please enter valid numbers in all fields.")

root = tk.Tk()
root.title("SWP Calculator")

tk.Label(root, text="Initial Investment (₹):").grid(row=0, column=0, padx=10, pady=10)
initial_amount_entry = tk.Entry(root)
initial_amount_entry.grid(row=0, column=1, padx=10, pady=10)

tk.Label(root, text="Withdrawal Amount (₹):").grid(row=1, column=0, padx=10, pady=10)
withdrawal_amount_entry = tk.Entry(root)
withdrawal_amount_entry.grid(row=1, column=1, padx=10, pady=10)

tk.Label(root, text="Rate of Return (%):").grid(row=2, column=0, padx=10, pady=10)
rate_of_return_entry = tk.Entry(root)
rate_of_return_entry.grid(row=2, column=1, padx=10, pady=10)

tk.Label(root, text="Withdrawal Frequency:").grid(row=3, column=0, padx=10, pady=10)
frequency_var = tk.StringVar(value="Monthly")
tk.Radiobutton(root, text="Monthly", variable=frequency_var, value="Monthly").grid(row=3, column=1, sticky="w")
tk.Radiobutton(root, text="Yearly", variable=frequency_var, value="Yearly").grid(row=4, column=1, sticky="w")

tk.Label(root, text="Duration (Years):").grid(row=5, column=0, padx=10, pady=10)
duration_entry = tk.Entry(root)
duration_entry.grid(row=5, column=1, padx=10, pady=10)

calculate_button = tk.Button(root, text="Calculate", command=calculate_swp)
calculate_button.grid(row=6, column=0, columnspan=2, pady=10)

result_label = tk.Label(root, text="", justify="left")
result_label.grid(row=7, column=0, columnspan=2, padx=10, pady=10)

root.mainloop()
