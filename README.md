package pong.model;


import pong.event.Event;
import pong.event.EventService;
import pong.view.Assets;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;

/*
 * Logic for the Pong Game
 * Model class representing the "whole" game
 * Nothing visual here
 *
 */
public class Pong {
    Random rand = new Random();

    public static final double GAME_WIDTH = 600;
    public static final double GAME_HEIGHT = 400;
    public static final double BALL_SPEED_FACTOR = 1.1;
    public static final long HALF_SEC = 500_000_000;


    private int pointsLeft;
    private int pointsRight;

    private Ball ball;
    private Paddle rightPaddle;
    private Paddle leftPaddle;
    private final List<Wall> walls;

    public Pong(Ball ball, Paddle rightPaddle, Paddle leftPaddle, List<Wall> walls){
        this.ball = ball;
        this.rightPaddle = rightPaddle;
        this.leftPaddle = leftPaddle;
        this.walls = walls;

    }
    // TODO Constructor

    // --------  Game Logic -------------

    private long timeForLastHit = HALF_SEC;         // To avoid multiple collisions

    public void update(long now) {
        ball.move();
        Event event = new Event(Event.Type.BALL_HIT_PADDLE);

        /*if (ball.getX() > GAME_HEIGHT || ball.getX() < 0) {
            if (ball.getX() > 0){
                this.pointsLeft++;
            }
            else{
                this.pointsRight++;
            }
            ball.setX(300 - (Ball.BALL_WIDTH / 2));
            ball.setY(200 - (Ball.BALL_HEIGHT /2));
        }*/


        if (ball.intersect(walls.get(3))){
            this.pointsLeft++;
        } else if (ball.intersect(walls.get(2))){
            this.pointsRight++;
        }
        if (isTimer(timeForLastHit)) {
            rightPaddle.move();
            if (ball.intersect(rightPaddle)) {

                EventService.add(event);
                //ball.setDy(-(ball.getDy()));
                ball.setDx(-(ball.getDx()) * BALL_SPEED_FACTOR);
            }

            leftPaddle.move();
            if (ball.intersect(leftPaddle)) {
                EventService.add(event);
                //ball.setDy(-(ball.getDy()));
                ball.setDx(-(ball.getDx()) * BALL_SPEED_FACTOR);
            }
            timeForLastHit = HALF_SEC;
        }


        for(Wall w : walls){
            if (ball.intersect(w)){
                if (w.getDir() == Wall.Dir.HORIZONTAL){
                    ball.setDy(-ball.getDy());

                }else{              //vertical

                    ball.setX(300);
                    ball.setY(200);
                    ball.setDx(-8);
                    ball.setDy(-2);
                    int temp = rand.nextInt(2);
                    if (temp == 1) {
                        ball.setDx(-(ball.getDx()));

                    }
                    int temp2 = rand.nextInt(2);
                    if (temp2 == 1) {
                        ball.setDy(-(ball.getDy()));


                    }

                }
            }
            if (leftPaddle.intersect(w)){
                leftPaddle.setDy(-leftPaddle.getDy());
            }
            if (rightPaddle.intersect(w)){
                rightPaddle.setDy(-rightPaddle.getDy());
            }
        }


      // TODO Most game logic here, i.e. move paddles etc.
    }

    public boolean isTimer(long now){
        timeForLastHit = timeForLastHit - (HALF_SEC/100);
        return (timeForLastHit > 0);

    }


    // --- Used by GUI  ------------------------

    public List<IPositionable> getPositionables() {
        List<IPositionable> drawables = new ArrayList<>();
        // TODO
        drawables.add(rightPaddle);
        drawables.add(leftPaddle);
        drawables.add(ball);
        return drawables;
    }

    public int getPointsLeft() {
        return pointsLeft;
    }

    public int getPointsRight() {
        return pointsRight;
    }

    public void setSpeedRightPaddle(double dy) {
        // TODO
        rightPaddle.setDy(dy);
    }

    public void setSpeedLeftPaddle(double dy) {
        // TODO
        leftPaddle.setDy(dy);
    }





}
