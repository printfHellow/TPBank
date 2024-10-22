/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Main.java to edit this template
 */
package GUI;
import java.io.Console;
import java.io.IOException;
import java.util.Locale;
import java.util.Random;
import java.util.ResourceBundle;
import java.util.Scanner;

public class TPBank {

    private static final Scanner sc = new Scanner(System.in);

    public static void getWordLanguage(Locale locale, String key) {
        ResourceBundle words = ResourceBundle.getBundle("Language.messages", locale);
        String value = words.getString(key);
        System.out.print(value);
    }

    public static int checkInputIntLimit(int min, int max, Locale language) {
        while (true) {
            try {
                String input = sc.nextLine().trim();
                if (!input.matches("\\d+")) throw new NumberFormatException();
                int result = Integer.parseInt(input);
                if (result < min || result > max) throw new NumberFormatException();
                return result;
            } catch (NumberFormatException e) {
                getWordLanguage(language, "errCheckInputIntLimit");
                System.out.println();
            }
        }
    }

    public static String checkInputString(Locale language) {
        while (true) {
            String result = sc.nextLine().trim();
            if (result.isEmpty()) {
                getWordLanguage(language, "errCheckInputString");
                System.out.println();
            } else {
                return result;
            }
        }
    }

    public static long checkInputAccount(Locale language) {
        while (true) {
            String result = checkInputString(language);
            if (!result.matches("^\\d{10}$")) {
                getWordLanguage(language, "errCheckInputAccount");
                System.out.println();
            } else {
                return Long.parseLong(result);
            }
        }
    }

    public static boolean isValidPass(String pass, Locale language) {
        int lengthP = pass.length();
        if (lengthP < 8 || lengthP > 31) {
            getWordLanguage(language, "errCheckLengthPassword");
            System.out.println();
            return false;
        }
        int countDigit = 0;
        int countLetter = 0;
        for (char ch : pass.toCharArray()) {
            if (Character.isDigit(ch)) countDigit++;
            else if (Character.isLetter(ch)) countLetter++;
        }
        if (countDigit < 1 || countLetter < 1) {
            getWordLanguage(language, "errCheckAlphanumericPassword");
            System.out.println();
            return false;
        }
        return true;
    }

    public static String checkInputPass(Locale language) {
        while (true) {
            String password = readPasswordWithAsterisks(language);
            if (isValidPass(password, language)) return password;
        }
    }

    public static String readPasswordWithAsterisks(Locale language) {
        Console console = System.console();
        if (console != null) {
            // Đọc mật khẩu và hiển thị ký tự *
            char[] passwordArray = console.readPassword("Enter Password: ");
            return new String(passwordArray);
        } else {
            // Nếu không chạy được trong Console, dùng cách đọc từ System.in
            StringBuilder password = new StringBuilder();
            try {
                while (true) {
                    char ch = (char) System.in.read(); // Đọc từng ký tự
                    if (ch == '\n') break;  // Dừng khi nhấn Enter
                    if (ch == '\b' && password.length() > 0) {  // Xóa ký tự khi nhấn Backspace
                        password.deleteCharAt(password.length() - 1);
                        System.out.print("\b \b");  // Xóa ký tự trên console
                    } else {
                        password.append(ch);
                        System.out.print("*");  // Hiển thị *
                    }
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
            System.out.println();  // Xuống dòng sau khi nhập xong
            return password.toString();
        }
    }

    private static final char[] chars = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz".toCharArray();

    public static String captchaText() {
        Random random = new Random();
        StringBuilder captcha = new StringBuilder();
        for (int i = 0; i < 5; i++) {
            int capIndex = random.nextInt(chars.length);
            captcha.append(chars[capIndex]);
        }
        return captcha.toString();
    }

    public static boolean checkInputCaptcha(String captcha, Locale language) {
        System.out.println("Captcha: " + captcha);
        getWordLanguage(language, "enterCaptcha");
        String captchaIn = checkInputString(language);
        return captcha.equals(captchaIn);
    }

    public static void login(Locale language) {
    getWordLanguage(language, "enterAccountNumber");
    long accountNumber = checkInputAccount(language);
    getWordLanguage(language, "enterPassword");
    String passString = checkInputPass(language);

    while (true) {
        String captcha = captchaText();  // Sinh captcha mới mỗi lần lặp
        if (checkInputCaptcha(captcha, language)) {
            getWordLanguage(language, "loginSuccess");
            System.out.println("");
            return;
        } else {
            getWordLanguage(language, "errCaptchaIncorrect");
            System.out.println("");
        }
    }
}


    public static void main(String[] args) {
        Locale vietnamese = new Locale("vi");
        Locale english = Locale.ENGLISH;

        System.out.println("1. Vietnamese");
        System.out.println("2. English");
        System.out.println("3. Exit");
        System.out.print("Please choose one option: ");
        int choice = checkInputIntLimit(1, 3, english);

        switch (choice) {
            case 1:
                login(vietnamese);
                break;
            case 2:
                login(english);
                break;
            case 3:
                System.out.println("Goodbye!");
                break;
            default:
                System.out.println("Invalid choice!");
                break;
        }
    }
}




errCheckInputIntLimit = Invalid! Re-input: 
errCheckInputString = String must not be empty!
errCheckInputAccount = Account number must is a number and must have 10 digits!
errCheckLengthPassword = Password must be between 8 and 31 characters!
errCheckAlphanumericPassword = Password must be must be alphanumeric!
errCaptchaIncorrect = Captcha incorrect!
enterCaptcha = Enter captcha: 
enterAccountNumber = Account number: 
enterPassword = Password: 
loginSuccess = Login success!

errCheckInputIntLimit = Khong hop le! Vui long nhap lai: 
errCheckInputString = Chuoi khong the rong! Vui long nhap lai: 
errCheckInputAccount = So tai khoan phai la 1 so va phai co 10 chu so!
errCheckLengthPassword = Mat khau phai trong khoang 8-31 ky tu! 
errCheckAlphanumericPassword = Mat khau phai chua ky tu va so!
errCaptchaIncorrect = Captcha sai!
enterCaptcha = Nhap 1 ky tu captcha: 
enterAccountNumber = So tai khoan: 
enterPassword = Mat khau: 
loginSuccess = Dang nhap thanh cong!
