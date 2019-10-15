# Minesweeper
# By Calvin Probst calvin.probst@gmail.com
# https://github.com/calvinProbstSchool/probstChapter3work
# 32, 4

import random
from typing import Tuple

import pygame
import sys
import math
from pygame.locals import *

FPS = 30
ANIMATIONSPEED = 3
ROWSIZE = 4
NEWNUMBERS = (2, 4)
WINNINGNUMBER = 2048
BOXSIZE = 100
GAPSIZE = 3
TEXTGAP = 100
WINDOWSIZE = GAPSIZE + (GAPSIZE + BOXSIZE) * ROWSIZE + 2 * TEXTGAP
BOARDSIZE = GAPSIZE + (GAPSIZE + BOXSIZE) * ROWSIZE

DIRUP = "UP"
DIRRIGHT = "RIGHT"
DIRDOWN = "DOWN"
DIRLEFT = "LEFT"

BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
BRIGHTBLUE = (0, 50, 255)
TILECOLOR1 = (180, 218, 85)
TILECOLOR2 = (244, 32, 105)
DARKTURQUOISE = (3, 54, 73)

BGCOLOR = BRIGHTBLUE
BOARDBGCOLOR = DARKTURQUOISE
BOXCOLOR1 = TILECOLOR1
BOXCOLOR2 = TILECOLOR2
TEXTCOLOR = BLACK
SCORETEXT = WHITE



def main(highscore):
    global DISPLAYSURF, FPSCLOCK, BOXFONT, SCOREFONT
    pygame.init()
    pygame.mixer_music.load("theMessage.mp3")
    pygame.mixer_music.play(-1, 0.0)
    FPSCLOCK = pygame.time.Clock()
    DISPLAYSURF = pygame.display.set_mode((WINDOWSIZE, WINDOWSIZE))
    BOXFONT = pygame.font.Font("3Dventure.ttf", 30)
    board = newBoard()

    score = 0

    DISPLAYSURF.fill(BGCOLOR)
    pygame.draw.rect(DISPLAYSURF, BOARDBGCOLOR, (TEXTGAP, TEXTGAP, BOARDSIZE, BOARDSIZE))
    pygame.display.update()
    drawGameBoard(board, score, highscore)

    undoButton = BOXFONT.render("Undo", False, TEXTCOLOR, BOXCOLOR2)
    ngButton = BOXFONT.render("New Game", False, TEXTCOLOR, BOXCOLOR2)
    undoW, undoH = undoButton.get_size()
    ngW, ngH = ngButton.get_size()
    undoPos = ((WINDOWSIZE * 2 / 3) - undoW / 2, WINDOWSIZE - (TEXTGAP / 2) - ngH / 2)
    ngPos = ((WINDOWSIZE / 3) - ngW / 2, WINDOWSIZE - (TEXTGAP / 2) - ngH / 2)

    DISPLAYSURF.blit(undoButton, undoPos)
    DISPLAYSURF.blit(ngButton, ngPos)

    undoRect = undoButton.get_rect()
    ngRect = ngButton.get_rect()

    undoRect.left, undoRect.top, = undoPos
    ngRect.left, ngRect.top, = ngPos

    print(str(undoRect))
    print(str(undoPos))
    print(str(ngPos))
    print(str(ngRect))

    playingGame = True
    firstMove = True

    mousex = 0
    mousey = 0

    oldBoard = board

    while True:
        mouseDown = False
        keyPress = False
        keyDir = ""
        if score > highscore:
            highscore = score
        drawGameBoard(board, score, highscore)
        if not gameCheckBoard(board):
            playingGame = False

        else:
            playingGame = True

        # do the button blit here

        pygame.display.update()

        for event in pygame.event.get():
            if event.type == QUIT or (event.type == KEYUP and event.key == K_ESCAPE):
                pygame.quit()
                sys.exit()
            elif event.type == KEYUP:
                if event.key == K_LEFT:
                    keyPress = True
                    keyDir = DIRLEFT
                elif event.key == K_RIGHT:
                    keyPress = True
                    keyDir = DIRRIGHT
                elif event.key == K_UP:
                    keyPress = True
                    keyDir = DIRUP
                elif event.key == K_DOWN:
                    keyPress = True
                    keyDir = DIRDOWN
            elif event.type == MOUSEMOTION:
                mousex, mousey = event.pos
            elif event.type == MOUSEBUTTONUP:
                mousex, mousey = event.pos
                mouseDown = True

        if mouseDown:
            if undoRect.collidepoint(mousex, mousey) and not firstMove:
                board = oldBoard
                drawGameBoard(board, score, highscore)
            elif ngRect.collidepoint(mousex, mousey):
                main(highscore)
        elif keyPress and playingGame and not keyDir == "":
            if gameCheckDir(board, keyDir):
                firstMove = False
                oldBoard = []
                for x in board:
                    col = []
                    for y in x:
                        col.append(y)
                    oldBoard.append(col)
                board, score = changeBoard(board, score, keyDir)
                board = newTile(board)


def changeBoard(board, score, dir):
    changedTiles = []
    for x in range(ROWSIZE):
        row = []
        for y in range(ROWSIZE):
            row.append(False)
        changedTiles.append(row)
    if dir == DIRRIGHT:
        for y in range(ROWSIZE):
            for x in range(ROWSIZE - 1, -1, -1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRLEFT, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not  changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesIn(board, DIRRIGHT, changedTiles), score)
    if dir == DIRDOWN:
        for x in range(ROWSIZE):
            for y in range(ROWSIZE - 1, -1, -1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRUP, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not  changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesIn(board, DIRDOWN, changedTiles), score)
    elif dir == DIRLEFT:
        for y in range(ROWSIZE):
            for x in range(0, ROWSIZE - 1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRRIGHT, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not  changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesIn(board, DIRLEFT, changedTiles), score)
    elif dir == DIRUP:
        for x in range(ROWSIZE):
            for y in range(0, ROWSIZE - 1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRDOWN, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not  changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesIn(board, DIRUP, changedTiles), score)


def changeBoardNoAnim(boardStaySame, score, dir):
    changedTiles = []
    for x in range(ROWSIZE):
        row = []
        for y in range(ROWSIZE):
            row.append(False)
        changedTiles.append(row)
    board = []
    for x in range(ROWSIZE):
        row = []
        for y in range(ROWSIZE):
            row.append(boardStaySame[x][y])
        board.append(row)
    if dir == DIRRIGHT:
        for y in range(ROWSIZE):
            for x in range(ROWSIZE - 1, -1, -1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRLEFT, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesInNoAnim(board, DIRRIGHT, changedTiles), score)
    if dir == DIRDOWN:
        for x in range(ROWSIZE):
            for y in range(ROWSIZE - 1, -1, -1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRUP, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not  changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesInNoAnim(board, DIRDOWN, changedTiles), score)
    elif dir == DIRLEFT:
        for y in range(ROWSIZE):
            for x in range(0, ROWSIZE - 1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRRIGHT, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not  changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesInNoAnim(board, DIRLEFT, changedTiles), score)
    elif dir == DIRUP:
        for x in range(ROWSIZE):
            for y in range(0, ROWSIZE - 1):
                boxBefore, beforeX, beforeY = getBoxInDir(DIRDOWN, x, y, board)
                box = board[x][y]
                if box == boxBefore and (not box == 0) and (not  changedTiles[beforeX][beforeY]) and (not changedTiles[x][y]):
                    board[x][y] = 0
                    board[beforeX][beforeY] *= 2
                    score += board[beforeX][beforeY]
                    changedTiles[beforeX][beforeY] = True
        return (pushBoxesInNoAnim(board, DIRUP, changedTiles), score)


def pushBoxesIn(board, dir, changes):
    if dir == DIRUP:
        for x in range(ROWSIZE):
            for y in range(ROWSIZE - 1, -1, -1):
                if board[x][y] == 0:
                    for belowY in range(y + 1, ROWSIZE):
                        board[x][belowY - 1] = board[x][belowY]
                        if changes[x][belowY]:
                            changes[x][belowY] = False
                            changes[x][belowY - 1] = True
                            chaChaSlideAnimation(int(board[x][belowY] / 2), x, belowY, DIRUP)
                        elif not board[x][belowY] == 0:
                            chaChaSlideAnimation(board[x][belowY], x, belowY, DIRUP)
                    board[x][ROWSIZE - 1] = 0
        return board
    elif dir == DIRLEFT:
        for y in range(ROWSIZE):
            for x in range(ROWSIZE - 1, -1, -1):
                if board[x][y] == 0:
                    for belowX in range(x + 1, ROWSIZE):
                        board[belowX - 1][y] = board[belowX][y]
                        if changes[belowX][y]:
                            changes[belowX][y] = False
                            changes[belowX - 1][y] = True
                            chaChaSlideAnimation(int(board[belowX][y] / 2), belowX, y, DIRLEFT)
                        elif not board[belowX][y] == 0:
                            chaChaSlideAnimation(board[belowX][y], belowX, y, DIRLEFT)
                    board[ROWSIZE - 1][y] = 0
        return board
    elif dir == DIRDOWN:
        for x in range(ROWSIZE):
            for y in range(ROWSIZE):
                if board[x][y] == 0:
                    for belowY in range(y - 1, -1, -1):
                        board[x][belowY + 1] = board[x][belowY]
                        if changes[x][belowY]:
                            changes[x][belowY] = False
                            changes[x][belowY + 1] = True
                            chaChaSlideAnimation(int(board[x][belowY] / 2), x, belowY, DIRDOWN)
                        elif not board[x][belowY] == 0:
                            chaChaSlideAnimation(board[x][belowY], x, belowY, DIRDOWN)
                    board[x][0] = 0
        return board
    elif dir == DIRRIGHT:
        for y in range(ROWSIZE):
            for x in range(ROWSIZE):
                if board[x][y] == 0:
                    for belowX in range(x - 1, -1, -1):
                        board[belowX + 1][y] = board[belowX][y]
                        if changes[belowX][y]:
                            changes[belowX][y] = False
                            changes[belowX + 1][y] = True
                            chaChaSlideAnimation(int(board[belowX][y] / 2), belowX, y, DIRRIGHT)
                        elif not board[belowX][y] == 0:
                            chaChaSlideAnimation(board[belowX][y], belowX, y, DIRRIGHT)
                    board[0][y] = 0
        return board


def pushBoxesInNoAnim(board, dir, changes):
    if dir == DIRUP:
        for x in range(ROWSIZE):
            for y in range(ROWSIZE - 1, -1, -1):
                if board[x][y] == 0:
                    for belowY in range(y + 1, ROWSIZE):
                        board[x][belowY - 1] = board[x][belowY]
                    board[x][ROWSIZE - 1] = 0
        return board
    elif dir == DIRLEFT:
        for y in range(ROWSIZE):
            for x in range(ROWSIZE - 1, -1, -1):
                if board[x][y] == 0:
                    for belowX in range(x + 1, ROWSIZE):
                        board[belowX - 1][y] = board[belowX][y]
                    board[ROWSIZE - 1][y] = 0
        return board
    elif dir == DIRDOWN:
        for x in range(ROWSIZE):
            for y in range(ROWSIZE):
                if board[x][y] == 0:
                    for belowY in range(y - 1, -1, -1):
                        board[x][belowY + 1] = board[x][belowY]
                    board[x][0] = 0
        return board
    elif dir == DIRRIGHT:
        for y in range(ROWSIZE):
            for x in range(ROWSIZE):
                if board[x][y] == 0:
                    for belowX in range(x - 1, -1, -1):
                        board[belowX + 1][y] = board[belowX][y]
                    board[0][y] = 0
        return board


def getBoxInDir(dir, x, y, board):
    if dir == DIRRIGHT:
        for lookX in range(x + 1, ROWSIZE):
            if not board[lookX][y] == 0:
                return board[lookX][y], lookX, y
    elif dir == DIRDOWN:
        for lookY in range(y + 1, ROWSIZE):
            if not board[x][lookY] == 0:
                return board[x][lookY], x, lookY
    elif dir == DIRLEFT:
        for lookX in range(x - 1, -1, -1):
            if not board[lookX][y] == 0:
                return board[lookX][y], lookX, y
    elif dir == DIRUP:
        for lookY in range(y - 1, -1, -1):
            if not board[x][lookY] == 0:
                return board[x][lookY], x, lookY
    return 0, x, y


def newBoard():
    board = []
    for x in range(ROWSIZE):
        row = []
        for y in range(ROWSIZE):
            row.append(0)
        board.append(row)
    board = newTile(board)
    return board


def gameCheckDir(board, dir):
    newBoard, randoScore = changeBoardNoAnim(board, 0, dir)
    print(str(board))
    print(str(newBoard))
    if board == newBoard:
        return False
    else:
        return True


def gameCheckBoard(board):
    moves = False
    for x in range(ROWSIZE):
        for y in range(ROWSIZE):
            if not board[x][y] == 0:
                if getBoxInDir(DIRUP, x, y, board)[0] == board[x][y] or getBoxInDir(DIRDOWN, x, y, board)[0] == board[x][y] or getBoxInDir(DIRRIGHT, x, y, board)[0] == board[x][y] or getBoxInDir(DIRLEFT, x, y, board)[0] == board[x][y]:
                    moves = True
            else:
                moves = True
    return moves


def drawBlankGameBoard():
    DISPLAYSURF.fill(BGCOLOR)
    pygame.draw.rect(DISPLAYSURF, BOARDBGCOLOR, (TEXTGAP, TEXTGAP, BOARDSIZE, BOARDSIZE))


def drawGameBoard(board, score, highscore):
    drawBlankGameBoard()
    for x in range(ROWSIZE):
        for y in range(ROWSIZE):
            drawTile(board[x][y], boardToCoord(x), boardToCoord(y))
    scoreLabel = BOXFONT.render("Score:" + str(score), True, BLACK)
    scoreW, scoreH = scoreLabel.get_size()
    DISPLAYSURF.blit(scoreLabel, (WINDOWSIZE / 3 - scoreW / 2, TEXTGAP / 2 - scoreH))
    highLabel = BOXFONT.render("Highscore: " + str(highscore), True, BLACK)
    highW, highH = highLabel.get_size()
    DISPLAYSURF.blit(highLabel, (WINDOWSIZE / 3 - highW / 2, TEXTGAP / 2 + highH / 2))
    undoButton = BOXFONT.render("Undo", False, TEXTCOLOR, BOXCOLOR2)
    ngButton = BOXFONT.render("New Game", False, TEXTCOLOR, BOXCOLOR2)
    undoW, undoH = undoButton.get_size()
    ngW, ngH = ngButton.get_size()
    undoPos = ((WINDOWSIZE * 2 / 3) - undoW / 2, WINDOWSIZE - (TEXTGAP / 2) - ngH / 2)
    ngPos = ((WINDOWSIZE / 3) - ngW / 2, WINDOWSIZE - (TEXTGAP / 2) - ngH / 2)

    DISPLAYSURF.blit(undoButton, undoPos)
    DISPLAYSURF.blit(ngButton, ngPos)


def drawBoardBase():
    pygame.draw.rect(DISPLAYSURF, BOARDBGCOLOR, ((TEXTGAP, TEXTGAP, BOARDSIZE, BOARDSIZE)))


def drawTile(tile, x, y):
    pygame.draw.rect(DISPLAYSURF, getColorValue(tile), (x, y, BOXSIZE, BOXSIZE))
    tileLabel = BOXFONT.render(str(tile), True, BOARDBGCOLOR)
    tileTextW, tileTextH = tileLabel.get_size()
    DISPLAYSURF.blit(tileLabel, ((x + (BOXSIZE / 2) - tileTextW / 2), (y + (BOXSIZE / 2) - tileTextH / 2)))


def getColorValue(tileValue):
    if tileValue == 0:
        return BOARDBGCOLOR
    else:
        step = math.log(tileValue, 2) - 1
        colorR = BOXCOLOR1[0] + int(step * ((BOXCOLOR2[0] - BOXCOLOR1[0]) / 10))
        colorB = BOXCOLOR1[1] + int(step * ((BOXCOLOR2[1] - BOXCOLOR1[1]) / 10))
        colorG = BOXCOLOR1[2] + int(step * ((BOXCOLOR2[2] - BOXCOLOR1[2]) / 10))
        colorFinal = (colorR, colorB, colorG)
        return (colorFinal)


def boardToCoord(num):
    return TEXTGAP + GAPSIZE + num * (GAPSIZE + BOXSIZE)


def boardToCenterCoord(num):
    return boardToCoord(num) + BOXSIZE / 2


def coordToBoard(num):
    if num < GAPSIZE + TEXTGAP:
        return -1
    overlap = (num - GAPSIZE - TEXTGAP) % (BOXSIZE + GAPSIZE)
    if overlap > BOXSIZE:
        return -1
    else:
        return (num - overlap - GAPSIZE - TEXTGAP) / (BOXSIZE + GAPSIZE)


def chaChaSlideAnimation(tileVal, boardX, boardY, moveDir):
    startX = boardToCoord(boardX)
    startY = boardToCoord(boardY)
    travelDistance = BOXSIZE + GAPSIZE
    stepSizeX = int(travelDistance / ANIMATIONSPEED)
    stepSizeY = int(travelDistance / ANIMATIONSPEED)
    xGS = 0
    yGS = 0
    if moveDir == DIRUP:
        stepSizeX = 0
        stepSizeY = -1 * stepSizeY
        yGS = 1 * GAPSIZE
    elif moveDir == DIRDOWN:
        stepSizeX = 0
        yGS = -1 * GAPSIZE
    elif moveDir == DIRLEFT:
        stepSizeX = -1 * stepSizeX
        stepSizeY = 0
        xGS = 1 * GAPSIZE
    elif moveDir == DIRRIGHT:
        stepSizeY = 0
        xGS = -1 * GAPSIZE
    for step in range(ANIMATIONSPEED + 1):
        drawTile(0, startX, startY)
        drawTile(0, startX + xGS, startY + yGS)
        drawTile(tileVal, (startX + (stepSizeX * step)), (startY + (stepSizeY * step)))
        pygame.display.update()
        FPSCLOCK.tick(FPS)


def newTile(board):
    tileProb = random.randint(1, 10)
    if tileProb == 1:
        tileVal = 4
    else:
        tileVal = 2
    placed = False
    while not placed:
        posX = random.randint(0, ROWSIZE - 1)
        posY = random.randint(0, ROWSIZE - 1)
        if board[posX][posY] == 0:
            board[posX][posY] = tileVal
            placed = True
    return board

main(0)
