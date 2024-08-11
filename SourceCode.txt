#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
#include <cctype>
#include <cmath>

using namespace std;

void Start();

enum enNumberConversion { Decimal = 1, Binary = 2, Octal = 3, HexaDecimal = 4 };

struct stNumberThatWillConvert
{
	string Decimal;
	string Binary;
	string Octal;
	string HexDecimal;
};

struct stOperationInfo
{
	enNumberConversion ConvertFrom;
	enNumberConversion ConvertTo;
	stNumberThatWillConvert NumberWillConvert;
	string Result;
};

short ReadUserOption()
{
	short Choice = 0;
	cout << "Choose Option to preforme [1] : [4] ?";
	cin >> Choice;

	// Check is valid input
	while (cin.fail())
	{
		cin.clear();
		cin.ignore(numeric_limits<streamsize>::max(), '\n');
		cout << "\nInvalid Number, Please enter a valid one [1] : [4] ?";
		cin >> Choice;
	}

	// Check is choice in range
	while (Choice < 1 || Choice > 4)
	{
		cout << "\nInvalid Choice Please enter number [1] : [4] ?";
		cin >> Choice;
	}
	return Choice;
}

bool IsDecimalNumber(string Number)
{
	if (Number.empty())
		return false;

	for (short i = 0; i < Number.length(); i++)
	{
		if (!(isdigit(Number[i]) || Number[i] == '-' || Number[i] == '.'))
			return false;
	}
	return true;
}

bool IsBinaryNumber(string Number)
{

	if (Number.empty())
		return false;
	for (char C : Number)
	{
		if (C != '0' && C != '1' && C != '-' && C != '.')
			return false;
	}
	return true;
}

bool IsOctalNumber(string Number)
{
	if (Number.empty())
		return false;

	for (short i = 0; i < Number.length(); i++)
	{
		if (Number[i] < '0' || Number[i] > '7')
			return false;
	}
	return true;
}

bool IsHexaDecimalNumber(string Numer)
{
	if (Numer.empty())
		return false;

	// Check for Optional prefix 0x || 0X
	if (Numer.size() >= 2 && Numer[0] == '0' && (Numer[1] == 'x' || Numer[1] == 'X'))
	{
		Numer = Numer.substr(2);
	}

	for(char C : Numer)
	{
		if (!(isxdigit(C) || C == '.' || C == '-'))
			return false;
	}
	return true;
}

bool IsValidInput(string Number , enNumberConversion ConversionType)
{
	switch (ConversionType)
	{
	case Decimal:
		if (IsDecimalNumber(Number))
			return true;
		break;

	case Binary:
		if (IsBinaryNumber(Number))
			return true;
		break;

	case HexaDecimal:
		if (IsHexaDecimalNumber(Number))
			return true; 
		break;

	case Octal:
		if (IsOctalNumber(Number))
			return true;
		break;
	}
	return false;
}

void GoBackToMainScreen()
{
	cout << "\nPress any key to go back !.";
	system("pause>0");
	Start();
}

void ShowWrongInputScreen()
{
	system("cls");
	cout << "============================================\n";
	cout << " Invalid input Please Try again !.\n";
	cout << "============================================\n";
	GoBackToMainScreen();
}

stNumberThatWillConvert ReadDecimalNumber()
{
	stNumberThatWillConvert NumberWillConvert;

	cout << "\nEnter a Decimal Number to Convert ?";
	cin.ignore(); // clear the newline left in the buffer from previous input
	getline(cin,NumberWillConvert.Decimal);

	if (IsValidInput(NumberWillConvert.Decimal, Decimal))
		return NumberWillConvert;
	else
		ShowWrongInputScreen();
}

stNumberThatWillConvert ReadBinaryNumber()
{
	stNumberThatWillConvert NumberWillConvert;

	cout << "\nEnter a Binary Number to Convert ?";
	cin.ignore(); // clear the newline left in the buffer from previous input
	getline(cin,NumberWillConvert.Binary);

	if (IsValidInput(NumberWillConvert.Binary, Binary))
		return NumberWillConvert;
	else
		ShowWrongInputScreen();
}

stNumberThatWillConvert ReadHexaDecimalNumber()
{
	stNumberThatWillConvert NumberWillConvert;

	cout << "\nEnter a HexaDecimal Number to Convert ?";
	cin.ignore(); // clear the newline left in the buffer from previous input
	getline(cin, NumberWillConvert.HexDecimal);

	if (IsValidInput(NumberWillConvert.HexDecimal, HexaDecimal))
		return NumberWillConvert;
	else
		ShowWrongInputScreen();
}

stNumberThatWillConvert ReadOctalNumber()
{
	stNumberThatWillConvert NumberWillConvert;

	cout << "\nEnter a Octal Number to Convert ?";
	cin.ignore(); // clear the newline left in the buffer from previous input
	getline(cin,NumberWillConvert.Octal);

	if (IsValidInput(NumberWillConvert.Octal, Octal))
		return NumberWillConvert;
	else
		ShowWrongInputScreen();
}

enNumberConversion ShowConversionOptions(string Mesage)
{
	system("cls");
	cout << "====================================================\n";
	cout << "\t" << Mesage << endl;
	cout << "====================================================\n";
	cout << "\t[1] Decimal.\n";
	cout << "\t[2] Binary.\n";
	cout << "\t[3] Octal.\n";
	cout << "\t[4] HexaDecimal.\n";
	cout << "====================================================\n";
	return (enNumberConversion)ReadUserOption();
}

stNumberThatWillConvert ReadNumberToConvert(stOperationInfo OperationInfo)
{
	stNumberThatWillConvert NumberWillConvert;

	if (OperationInfo.ConvertFrom == enNumberConversion::Decimal)
		NumberWillConvert = ReadDecimalNumber();

	if (OperationInfo.ConvertFrom == enNumberConversion::Binary)
		NumberWillConvert = ReadBinaryNumber();


	if (OperationInfo.ConvertFrom == enNumberConversion::HexaDecimal)
		NumberWillConvert = ReadHexaDecimalNumber();

	if (OperationInfo.ConvertFrom == enNumberConversion::Octal)
		NumberWillConvert = ReadOctalNumber();

	return NumberWillConvert;
}

int HexaDigitToDecimal(char Digit)
{
	if (Digit >= '0' && Digit <= '9') return Digit - '0';
	if (Digit >= 'A' && Digit <= 'F') return Digit - 'A' + 10;
	if (Digit >= 'a' && Digit <= 'f') return Digit - 'a' + 10;
	return 0;
}

string DecimalToBinary(double Decimal)
{
	bool IsNegative = Decimal < 0;
	Decimal = std::abs(Decimal); // Use absolute value for conversion

	int IntegerPart = static_cast<int>(Decimal);
	double FractionPart = Decimal - IntegerPart;

	string result;

	if (IntegerPart == 0)
		result = "0";
	else 
	{
		while (IntegerPart > 0) 
		{
			result = to_string(IntegerPart % 2) + result;
			IntegerPart /= 2;
		}
	}

	if (FractionPart > 0) 
	{
		result += '.';
		short precisionCount = 0; // To avoid infinite loops in some cases
		while (FractionPart > 0 && precisionCount < 32) // Limit precision to 32 bits for practical purposes
		{ 
			FractionPart *= 2;
			int bit = static_cast<int>(FractionPart);
			result += to_string(bit);
			FractionPart -= bit;
			precisionCount++;
		}
	}
	return IsNegative ? "-" + result : result;
}

string BinaryToDecimal(string binary)
{
	bool isNegative = binary[0] == '-';
	if (isNegative)
	{
		binary = binary.substr(1);
	}

	size_t pointPos = binary.find('.');
	string integerPart = (pointPos == string::npos) ? binary : binary.substr(0, pointPos);
	string fractionPart = (pointPos == string::npos) ? "" : binary.substr(pointPos + 1);

	double decimalValue = 0;
	int power = 0;

	// Handle integer part
	for (short i = integerPart.length() - 1; i >= 0; --i)
	{
		if (binary[i] == '1')
			decimalValue += pow(2, power);
		power++;
	}

	power = -1;
	// Handle fraction part
	for (char c : fractionPart)
	{
		if (c == '1')
			decimalValue += pow(2, power);
		power--;
	}

	string result = to_string(decimalValue);
	return isNegative ? '-' + result : result;
}

string DecimalToHexadecimal(double Decimal)
{
	bool IsNegative = Decimal < 0;
	Decimal = std::abs(Decimal); // Use absolute value for conversion

	int IntegerPart = static_cast<int>(Decimal);
	double FractionPart = Decimal - IntegerPart;

	string hexCharacters = "0123456789ABCDEF";
	string result;

	if (IntegerPart == 0)
		result = "0";
	else
	{
		while (IntegerPart > 0)
		{
			result = hexCharacters[IntegerPart % 16] + result;
			IntegerPart /= 16;
		}
	}

	if (FractionPart > 0)
	{
		result += '.';
		short precisionCount = 0; // To avoid infinite loops in some cases
		while (FractionPart > 0 && precisionCount < 8) { // Limit precision to 8 bits for practical purposes
			FractionPart *= 16;
			int digit = static_cast<int>(FractionPart);
			result += hexCharacters[digit];
			FractionPart -= digit;
			precisionCount++;
		}
	}
	return IsNegative ? "-" + result : result;
}

string HexadecimalToDecimal(string hex)
{
	bool isNegative = hex[0] == '-';
	if (isNegative)
	{
		hex = hex.substr(1);
	}

	size_t pointPos = hex.find('.');
	string integerPart = (pointPos == string::npos) ? hex : hex.substr(0, pointPos);
	string fractionPart = (pointPos == string::npos) ? "" : hex.substr(pointPos + 1);

	double decimalValue = 0;
	int power = 0;

	// Handle integer part
	for (short i = integerPart.length() - 1; i >= 0; --i)
	{
		decimalValue += HexaDigitToDecimal(integerPart[i]) * pow(16, power);
		power++;
	}

	power = -1;
	// Handle fraction part
	for (short i = 0; i < fractionPart.length(); ++i)
	{
		decimalValue += HexaDigitToDecimal(fractionPart[i]) * pow(16, power);
		power--;
	}

	return to_string(isNegative ? -decimalValue : decimalValue);
}

string DecimalToOctal(double Decimal)
{
	bool IsNegative = Decimal < 0;
	Decimal = std::abs(Decimal); // Use absolute value for conversion

	int IntegerPart = static_cast<int>(Decimal);
	double FractionPart = Decimal - IntegerPart;

	string result;

	if (IntegerPart == 0)
		result = "0";
	else
	{
		while (IntegerPart > 0)
		{
			result = to_string(IntegerPart % 8) + result;
			IntegerPart /= 8;
		}
	}

	if (FractionPart > 0)
	{
		result += '.';
		short precisionCount = 0; // To avoid infinite loops in some cases
		while (FractionPart > 0 && precisionCount < 8) { // Limit precision to 8 bits for practical purposes
			FractionPart *= 8;
			int digit = static_cast<int>(FractionPart);
			result += to_string(digit);
			FractionPart -= digit;
			precisionCount++;
		}
	}
	return IsNegative ? "-" + result : result;
}

string OctalToDecimal(string octal)
{
	bool isNegative = octal[0] == '-';
	if (isNegative)
	{
		octal = octal.substr(1);
	}

	size_t pointPos = octal.find('.');
	string integerPart = (pointPos == string::npos) ? octal : octal.substr(0, pointPos);
	string fractionPart = (pointPos == string::npos) ? "" : octal.substr(pointPos + 1);

	double decimalValue = 0;
	int power = 0;

	// Handle integer part
	for (short i = integerPart.length() - 1; i >= 0; --i)
	{
		decimalValue += (integerPart[i] - '0') * pow(8, power);
		power++;
	}

	power = -1;
	// Handle fraction part
	for (short i = 0; i < fractionPart.length(); ++i)
	{
		decimalValue += (fractionPart[i] - '0') * pow(8, power);
		power--;
	}

	return to_string(isNegative ? -decimalValue : decimalValue);
}

void PrintResult(stOperationInfo OperationInfo)
{
	system("cls");
	cout << "=============================\n";
	cout << "\tResult : \n";
	cout << "=============================\n";
	cout << "\t" << OperationInfo.Result << endl;
	cout << "=============================\n";
}

stOperationInfo PerformConversionOption(stOperationInfo OperationInfo)
{
	OperationInfo.ConvertFrom = ShowConversionOptions("Choose the base to convert from");
	OperationInfo.ConvertTo = ShowConversionOptions("Choose the base to convert to");

	// Avoid re-entering the conversion type screen if from and to conversion types are same.
	while (OperationInfo.ConvertTo == OperationInfo.ConvertFrom)
	{
		cout << "\nThe 'Convert from' and 'Convert to' options cannot be the same. Please choose different options.\n";
		OperationInfo.ConvertTo = ShowConversionOptions("Choose the base to convert to");
	}

	OperationInfo.NumberWillConvert = ReadNumberToConvert(OperationInfo);

	string result;
	switch (OperationInfo.ConvertFrom)
	{
	case Decimal:
		if (OperationInfo.ConvertTo == Binary)
			result = DecimalToBinary(stod(OperationInfo.NumberWillConvert.Decimal));
		if (OperationInfo.ConvertTo == Octal)
			result = DecimalToOctal(stod(OperationInfo.NumberWillConvert.Decimal));
		if (OperationInfo.ConvertTo == HexaDecimal)
			result = DecimalToHexadecimal(stod(OperationInfo.NumberWillConvert.Decimal));
		break;

	case Binary:
		if (OperationInfo.ConvertTo == Decimal)
			result = BinaryToDecimal(OperationInfo.NumberWillConvert.Binary);
		if (OperationInfo.ConvertTo == Octal)
			result = DecimalToOctal(stod(BinaryToDecimal(OperationInfo.NumberWillConvert.Binary)));
		if (OperationInfo.ConvertTo == HexaDecimal)
			result = DecimalToHexadecimal(stod(BinaryToDecimal(OperationInfo.NumberWillConvert.Binary)));
		break;

	case Octal:
		if (OperationInfo.ConvertTo == Decimal)
			result = OctalToDecimal(OperationInfo.NumberWillConvert.Octal);
		if (OperationInfo.ConvertTo == Binary)
			result = DecimalToBinary(stod(OctalToDecimal(OperationInfo.NumberWillConvert.Octal)));
		if (OperationInfo.ConvertTo == HexaDecimal)
			result = DecimalToHexadecimal(stod(OctalToDecimal(OperationInfo.NumberWillConvert.Octal)));
		break;

	case HexaDecimal:
		if (OperationInfo.ConvertTo == Decimal)
			result = HexadecimalToDecimal(OperationInfo.NumberWillConvert.HexDecimal);
		if (OperationInfo.ConvertTo == Binary)
			result = DecimalToBinary(stod(HexadecimalToDecimal(OperationInfo.NumberWillConvert.HexDecimal)));
		if (OperationInfo.ConvertTo == Octal)
			result = DecimalToOctal(stod(HexadecimalToDecimal(OperationInfo.NumberWillConvert.HexDecimal)));
		break;
	}

	OperationInfo.Result = result;

	return OperationInfo;
}

void Start()
{
	char choice;
	do
	{
		system("cls");

		stOperationInfo OperationInfo;
		OperationInfo = PerformConversionOption(OperationInfo);
		PrintResult(OperationInfo);

		cout << "\nDo you want to perform another conversion? (y/n): ";
		cin >> choice;

	} while (choice == 'y' || choice == 'Y');
}

int main()
{
	Start();
	system("pause>0");

	return 0;
}
