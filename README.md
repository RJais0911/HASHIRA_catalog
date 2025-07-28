#include <iostream>
#include <vector>
#include <cmath>
#include <sstream>

// Function to decode the base-N string into an integer
long long decodeBase(const std::string& value, int base) {
    long long result = 0;
    for (size_t i = 0; i < value.size(); ++i) {
        int digit = (value[i] >= '0' && value[i] <= '9') ? (value[i] - '0') : (value[i] - 'a' + 10);
        result = result * base + digit;
    }
    return result;
}

// Lagrange interpolation to compute constant term (c)
long long lagrangeInterpolation(const std::vector<int>& x, const std::vector<long long>& y, int k) {
    long long c = 0;

    for (int i = 0; i < k; ++i) {
        long long term = y[i];
        for (int j = 0; j < k; ++j) {
            if (i != j) {
                term *= (0 - x[j]); // Since we're evaluating at x = 0
                term /= (x[i] - x[j]);
            }
        }
        c += term;
    }

    return c;
}

int main() {
    // Test case 2 data
    std::vector<int> x = {1, 2, 3, 4, 5, 6, 7};
    std::vector<std::string> values = {
        "13444211440455345511", "aed7015a346d63", "6aeeb69631c227c",
        "e1b5e05623d881f", "316034514573652620673", "2122212201122002221120200210011020220200", 
        "20120221122211000100210021102001201112121"
    };
    std::vector<int> bases = {6, 15, 15, 16, 8, 3, 3};

    // Decoding the y-values
    std::vector<long long> y;
    for (size_t i = 0; i < values.size(); ++i) {
        long long decoded_value = decodeBase(values[i], bases[i]);
        y.push_back(decoded_value);
    }

    // Perform Lagrange interpolation to find the constant term (c)
    long long c = lagrangeInterpolation(x, y, 7);  // We use the first 7 roots

    // Output the result
    std::cout << "The constant term c is: " << c << std::endl;

    return 0;
}
