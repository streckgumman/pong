package pong.model;

import java.util.Random;

import static pong.model.Pong.GAME_HEIGHT;
import static pong.model.Pong.GAME_WIDTH;

/*
 * A Ball for the Pong game
 * A model class
 */
public class Ball extends AbstractMovable  {
     Random rand = new Random();

    private double height = BALL_WIDTH;
    private double width = BALL_HEIGHT;

    public static final double BALL_WIDTH = 40;
    public static final double BALL_HEIGHT = 40;


    public Ball(double x, double y, double width, double height, double dx, double dy) {
        super(x, y, width, height, dx, dy);

    }

    public Ball(double x, double y) {
        this(x, y, BALL_WIDTH, BALL_HEIGHT, -8, -2);
    }





    //TODO move
    public void move(){
        setX(getX() + getDx());
        setY(getY() + getDy());
    }

}
