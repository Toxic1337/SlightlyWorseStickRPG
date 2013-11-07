package slightlyworsestickrpg;

import org.lwjgl.LWJGLException;
import org.lwjgl.Sys;
import org.lwjgl.input.Keyboard;
import org.lwjgl.opengl.Display;
import org.lwjgl.opengl.DisplayMode;
import org.lwjgl.opengl.GL11;

public class Main {

    public static int x = -3500, y = -2700, delta, fps;
    float rotation = 0, speed = 0.2f;
    long lastFrame, lastFPS;
    public static boolean canL = false, canR = false, canU = false, canD = false, skateboard = false, l = false, u = false, r = false, d = false;
    public static boolean titlePage, game;
    public static String filePath = "G:\\MyPortfolio\\Netbeans\\SlightlyWorseStickRPG\\Extra\\";
    Map map = new Map();
    Player player = new Player();
    Stats stats = new Stats();
    StickCar car = new StickCar();
    CarRoutes route = new CarRoutes();
    TitleScreen title = new TitleScreen();
    UofStick uni = new UofStick();
    BlackJack bj = new BlackJack();

    public void start() {
        try {
            Display.setDisplayMode(new DisplayMode(800, 600));
            Display.create();
        } catch (LWJGLException e) {
            e.printStackTrace();
            System.exit(0);
        }

        //titlePage = true;
        stats.init();
        title.init();
        //initGL(); // init OpenGL
        getDelta(); // call once before loop to initialise lastFrame
        lastFPS = getTime(); // call before loop to initialise fps timer

        while (!Display.isCloseRequested()) {
            delta = getDelta();

            update(delta);
            renderGL();

            Display.update();
            Display.sync(60); // cap fps to 60fps
        }

        Display.destroy();
    }

    public void update(int delta) {

        player.checkRestrictions();
        player.CheckBuildings();

        if (!Stats.UofStickActive) {
            if (Keyboard.isKeyDown(Keyboard.KEY_LEFT)) {
                if (canL) {
                    x += speed * delta;
                    l = true;
                }
            }
            if (Keyboard.isKeyDown(Keyboard.KEY_RIGHT)) {
                if (canR) {
                    x -= speed * delta;
                    r = true;
                }
            }

            if (Keyboard.isKeyDown(Keyboard.KEY_UP)) {
                if (canU) {
                    y += speed * delta;
                    u = true;
                }
            }
            if (Keyboard.isKeyDown(Keyboard.KEY_DOWN)) {
                if (canD) {
                    y -= speed * delta;
                    d = true;
                }
            }

            if (Keyboard.isKeyDown(Keyboard.KEY_LSHIFT)) {
                skateboard = true;
                speed = 0.6f;
            }
            if (!Keyboard.isKeyDown(Keyboard.KEY_LSHIFT) && skateboard) {
                skateboard = false;
                speed = 0.2f;
            }

            if (Keyboard.isKeyDown(Keyboard.KEY_L)) {
                //Miscallaneous debug key
            }
            canL = true;
            canU = true;
            canR = true;
            canD = true;
        } else if (Stats.UofStickActive) {
            if (Keyboard.isKeyDown(Keyboard.KEY_ESCAPE)) {
                Stats.UofStickActive = false;
                y -= 10;
            }
        }

        updateFPS(); // update FPS Counter
    }

    public int getDelta() {
        long time = getTime();
        int delta = (int) (time - lastFrame);
        lastFrame = time;

        return delta;
    }

    public long getTime() {
        return (Sys.getTime() * 1000) / Sys.getTimerResolution();
    }

    public void updateFPS() {
        if (getTime() - lastFPS > 1000) {
            Display.setTitle("Stick RPG (FPS: " + fps + ")");
            fps = 0;
            lastFPS += 1000;
        }
        fps++;
    }

    public void initGL() {
        GL11.glMatrixMode(GL11.GL_PROJECTION);
        GL11.glLoadIdentity();
        GL11.glOrtho(0, 800, 600, 0, 1, -1);
        GL11.glMatrixMode(GL11.GL_MODELVIEW);
    }

    public void renderGL() {
        GL11.glClear(GL11.GL_COLOR_BUFFER_BIT | GL11.GL_DEPTH_BUFFER_BIT);
        // Reset The Current Modelview Matrix

        if (titlePage) {
            title.titleScreen();
        } else if (Stats.UofStickActive) {
            uni.UniHub();
            stats.HUD();
        } else if (//card game) {
        
        }
        else  {
            Stats.carIntStart++;

            map.drawBackground();

            if (skateboard) {
                player.drawSkateboard();
            }
            player.drawCharacter();

            if (Stats.carActive && stats.curRoutedir[stats.routePos] == stats.curRoutedir[1999]) {
                Stats.carIntStart = 2000;
            }

            if (Stats.carActive) {
                car.createCar(stats.curRoute[stats.routePos][0] + x, stats.curRoute[stats.routePos][1] + y, stats.curRoutedir[stats.routePos], stats.curCar[stats.currentCar][0], stats.curCar[stats.currentCar][1]);
            }

            stats.routePos++;
            Stats.gameInt++;

            if (Stats.gameInt >= fps) {
                stats.gameLength++;
                Stats.gameInt = 0;
            }

            if (stats.gameLength == stats.lastGameLength + 15) {
                stats.lastGameLength += 15;
                Stats.hourLength += (9.5f - 3.15f) / 12f;
            }

            stats.HUD();

        }
        l = false;
        r = false;
        u = false;
        d = false;
    }

    public static void main(String[] argv) {
        Main main = new Main();
        main.start();
    }
}
