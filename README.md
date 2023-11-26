#include <iostream>
#include <vector>
#include <cmath>
#include <algorithm>

struct DataPoint {
    double x, y; // مختصات
    std::string label; // برچسب
};

// محاسبه فاصله اقلیدسی بین دو نقطه
double calculateDistance(const DataPoint& a, const DataPoint& b) {
    return std::sqrt(std::pow(a.x - b.x, 2) + std::pow(a.y - b.y, 2));
}

// الگوریتم KNN
std::string knn(const std::vector<DataPoint>& dataset, const DataPoint& input, int k) {
    std::vector<std::pair<double, std::string>> distances;

    // محاسبه فاصله از نقطه ورودی تا هر نقطه دیگر
    for (const auto& point : dataset) {
        double distance = calculateDistance(input, point);
        distances.push_back(std::make_pair(distance, point.label));
    }

    // مرتب‌سازی فواصل به ترتیب صعودی
    std::sort(distances.begin(), distances.end());

    // شمارش تعداد هر برچسب
    std::map<std::string, int> labelCounter;
    for (int i = 0; i < k; ++i) {
        labelCounter[distances[i].second]++;
    }

    // یافتن برچسب پرتکرار
    int maxCount = 0;
    std::string resultLabel;
    for (const auto& pair : labelCounter) {
        if (pair.second > maxCount) {
            maxCount = pair.second;
            resultLabel = pair.first;
        }
    }

    return resultLabel;
}

int main() {
    std::vector<DataPoint> dataset = {
        {1.2, 3.5, "A"},
        {2.5, 1.4, "B"},
        {4.0, 6.5, "A"},
        {3.5, 2.8, "B"},
        // ...
    };

    DataPoint input = {2.8, 2.7, ""}; // نقطه ورودی برای دسته‌بندی

    int k = 3; // تعداد همسایه‌ها

    std::string result = knn(dataset, input, k);
    std::cout << "Predicted label: " << result << std::endl;

    return 0;
}
