import java.util.Locale;
import java.util.Random;
import java.util.ResourceBundle;
import java.util.Scanner;
import java.util.regex.Pattern;

public class TPBank {

    private static final Scanner sc = new Scanner(System.in);
    
    public static void getWordLanguage(Locale locate, String key){
        ResourceBundle words = ResourceBundle.getBundle("Language.messages", locate);
        String value = words.getString(key);
        System.out.print(value);
    }

    public static int checkInputIntLimit(int min, int max, Locale language){
        while (true) {
            try {
                int result = Integer.parseInt(sc.nextLine().trim());
                if (result < min || result > max) {
                    throw new NumberFormatException();
                }
                return result;
            } catch (NumberFormatException e) {
                getWordLanguage(language, "errCheckInputIntLimit");
                System.out.println("");
            }
        }
    }
    
    public static String checkInputString(Locale language){
        while (true){
            String result = sc.nextLine().trim();
            if (result.isEmpty()){
                getWordLanguage(language, "errCheckInputString");
                System.out.println("");
            } else {
                return result;
            }
        }
    } 
    
    public static int checkInputAccount(Locale language){
        String result;
        while (true){
            result = checkInputString(language);
            if (!result.matches("^\\d{10}$")){
                getWordLanguage(language, "errCheckInputAccount");
                System.out.println("");
            } else {
                return Integer.parseInt(result);
            }
        }
    }
    
    public static boolean isValidPass(String pass, Locale language){
        int lengthP = pass.length();
        if (lengthP < 8 || lengthP > 31){
            getWordLanguage(language, "errCheckLengthPassword");
            System.out.println("");
            return false;
        }
        int countDigit = 0;
        int countLetter = 0;
        for (int i=0; i<lengthP-1; i++){
            if (Character.isDigit(pass.charAt(i))) countDigit++;
            else if (Character.isLetter(pass.charAt(i))) countLetter++;
        }
        if (countDigit < 1 || countLetter < 1){
            getWordLanguage(language, "errCheckAlphanumericPassword");
            System.out.println("");
            return false;
        }
        return true;
    }
    
    public static String checkInputPass(Locale language){
        String result;
        while (true){
            result = checkInputString(language);
            if (isValidPass(result, language)) return result;
        }
    }
    
    // Removed chars array and used regex pattern directly
    private static final Pattern CHAR_PATTERN = Pattern.compile("[0-9A-Za-z]");

    public static String captchaText(){
        Random random = new Random();
        StringBuilder captcha = new StringBuilder();
        for (int i=0; i<5; i++){
            char randomChar;
            do {
                randomChar = (char) (random.nextInt(62) + '0');
            } while (!CHAR_PATTERN.matcher(String.valueOf(randomChar)).matches());
            captcha.append(randomChar);
        }
        return captcha.toString();
    }

    public static boolean checkInputCaptcha(String captcha, Locale language){
        System.out.println("Captcha: " + captcha);
        getWordLanguage(language, "enterCaptcha");
        String captchaIn = checkInputString(language);
        for (int i=0; i<captchaIn.length(); i++){
            if (!captcha.contains(Character.toString(captchaIn.charAt(i)))) return false;
        }
        return true;
    }

    public static void login(Locale language){
        getWordLanguage(language, "enterAccountNumber");
        int accountNumber = checkInputAccount(language);
        getWordLanguage(language, "enterPassword");
        String passString = checkInputPass(language);
        String captcha = captchaText();
        while (true){
            if (checkInputCaptcha(captcha, language)){
                getWordLanguage(language, "loginSuccess");
                System.out.println("");
                return;
            }else{
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
        System.out.print("Please choice one option: ");
        int choice = checkInputIntLimit(1, 3, english);
        switch (choice){
            case 1:
                login(vietnamese);
                break;
            case 2:  
                login(english);
                break;
            case 3:
                System.out.println("Goodbye!");
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
