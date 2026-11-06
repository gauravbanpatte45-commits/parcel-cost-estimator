# parcel-cost-estimator
"""
Parcel Cost Estimator
---------------------
A simple console program that estimates the cost of sending a parcel.

Formula:
    total cost = base fee + (weight x rate per kg) + (distance x rate per km)

IMPORTANT: The fees and rates below are EXAMPLE values only.
They are NOT real courier prices. No currency is assumed;
the numbers are just "currency units". Change them to suit your project.
"""

import math


# ----------------------------------------------------------------------
# Example pricing constants - change these numbers to test other prices
# ----------------------------------------------------------------------
BASE_FEE = 5.00       # fixed fee charged for every parcel
RATE_PER_KG = 1.20    # charge for each kilogram of parcel weight
RATE_PER_KM = 0.40    # charge for each kilometre of delivery distance


def read_number(prompt, field_name, allow_zero):
    """
    Ask the user for a number and keep asking until the answer is valid.

    prompt      - the question shown to the user
    field_name  - the name used in error messages, e.g. "Weight"
    allow_zero  - True if 0 is acceptable, False if it is not

    Returns the valid number as a float.
    """
    while True:
        text = input(prompt).strip()

        # Check 1: the user typed nothing
        if text == "":
            print("  Invalid input: please type a number.")
            continue

        # Check 2: the text must be a number (for example "2.5")
        try:
            value = float(text)
        except ValueError:
            print(f"  Invalid input: '{text}' is not a number. Please try again.")
            continue

        # Check 3: float() also accepts "nan" and "inf", which are not useful here
        if not math.isfinite(value):
            print("  Invalid input: please enter a normal number, not 'nan' or 'inf'.")
            continue

        # Check 4: no negative values
        if value < 0:
            print(f"  Invalid input: {field_name} cannot be negative.")
            continue

        # Check 5: zero is only allowed when allow_zero is True
        if value == 0 and not allow_zero:
            print(f"  Invalid input: {field_name} must be greater than zero.")
            continue

        # All checks passed. abs() removes any minus sign from zero,
        # so an input of "-0" is shown as 0.00 instead of -0.00.
        return abs(value)


def calculate_cost(weight_kg, distance_km):
    """
    Work out each part of the cost.

    Returns three numbers: (weight charge, distance charge, total cost).
    """
    weight_charge = weight_kg * RATE_PER_KG
    distance_charge = distance_km * RATE_PER_KM
    total = BASE_FEE + weight_charge + distance_charge
    return weight_charge, distance_charge, total


def show_breakdown(weight_kg, distance_km):
    """Print the cost breakdown in a neat table, to two decimal places."""
    weight_charge, distance_charge, total = calculate_cost(weight_kg, distance_km)

    print()
    print("Cost breakdown")
    print("-" * 26)
    print(f"{'Base fee:':<18}{BASE_FEE:>8.2f}")
    print(f"{'Weight charge:':<18}{weight_charge:>8.2f}   "
          f"({weight_kg:.2f} kg x {RATE_PER_KG:.2f} per kg)")
    print(f"{'Distance charge:':<18}{distance_charge:>8.2f}   "
          f"({distance_km:.2f} km x {RATE_PER_KM:.2f} per km)")
    print("-" * 26)
    print(f"{'Estimated total:':<18}{total:>8.2f}")
    print()
    print("Reminder: these are example rates, not real courier prices.")


def main():
    """Run the program: ask for the inputs, then show the cost."""
    print("=== Parcel Cost Estimator ===")
    print("Note: the rates in this program are examples, not real prices.")
    print()

    weight_kg = read_number("Enter parcel weight in kilograms: ", "Weight", allow_zero=False)
    distance_km = read_number("Enter delivery distance in kilometres: ", "Distance", allow_zero=True)

    show_breakdown(weight_kg, distance_km)


# This line makes main() run only when this file is started directly,
# not when another file imports it.
if __name__ == "__main__":
    main()