#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <sstream>

class Product {
public:
	std::string name;
	std::vector<std::string> ingredients;

	Product(std::string name, std::vector<std::string> ingredients) : name(name), ingredients(ingredients) {}

	bool isToxic(const std::vector<std::string>& toxicSubstances) const {
		for (const std::string& ingredient : ingredients) {
			for (const std::string& toxicSubstance : toxicSubstances) {
				if (ingredient == toxicSubstance) {
					return true;
				}
			}
		}
		return false;
	}
};

std::vector<std::string> splitString(const std::string& str, char delimiter) {
	std::vector<std::string> tokens;
	std::string token;
	std::istringstream tokenStream(str);
	while (std::getline(tokenStream, token, delimiter)) {
		tokens.push_back(token);
	}
	return tokens;
}

int main() {
	std::ifstream toxicFile("toxic_substances.txt");
	std::vector<std::string> toxicSubstances;
	std::string toxicSubstance;
	while (std::getline(toxicFile, toxicSubstance)) {
		toxicSubstances.push_back(toxicSubstance);
	}
	toxicFile.close();

	std::ifstream productFile("products.txt");
	std::string line;
	while (std::getline(productFile, line)) {
		std::string productName = line.substr(0, line.find(':'));
		std::string ingredientsStr = line.substr(line.find(':') + 2);
		std::vector<std::string> ingredients = splitString(ingredientsStr, ',');

		Product product(productName, ingredients);
		if (product.isToxic(toxicSubstances)) {
			std::cout << "Toxic product found: " << product.name << std::endl;
		}
	}
	productFile.close();

	return 0;
}
