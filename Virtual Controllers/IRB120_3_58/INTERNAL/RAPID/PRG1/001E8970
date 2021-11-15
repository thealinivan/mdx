
    MODULE Module1

    !===================================================
    !================== GLOBAL DATA ====================
    !===================================================
    
    ! TOOL DATA
    PERS tooldata pen;
    CONST num penWidth:= 163.48;
    CONST num traceTolerance:= 4;
    CONST num penHoverHeight:= 16;
    !tool dependencies
    CONST num penUp:= penWidth + penHoverHeight + traceTolerance;
    CONST num penDown:= penWidth + traceTolerance;
    
    ! WORK OBJECT DATA
    PERS wobjdata paper;
    VAR num workObjHeight:= 297;
    VAR num workObjWidth:= 420;
    VAR num woMarginHeight:= 20;
    VAR num woMarginWidth:= -7;
    
    ! POSITION DATA
    VAR robtarget workObjRef;
    VAR robtarget frameRef;
    VAR robtarget startElement;
    VAR robtarget penPos;
    CONST robtarget homePos:=[[302.02,-0.00,557.98],[4.10427E-5,-1.73313E-9,-1,-1.09285E-9],[-1,0,-1,0],[9E+9,9E+9,9E+9,9E+9,9E+9,9E+9]];
    CONST robtarget woOrgPos:= [[200.00,210,penUp],[4.06056E-5,-7.54147E-8,-1,-1.40576E-7],[0,0,0,0],[9E+9,9E+9,9E+9,9E+9,9E+9,9E+9]];
    CONST robtarget modelStartPos:=[[220, 203, penUp],[0.000793771,-0.0908509,-0.995802,0.0111497],[0,-1,0,0],[9E+9,9E+9,9E+9,9E+9,9E+9,9E+9]];
   
    
    ! SPEED DATA
    CONST speeddata ultraLowSpeed:= v10;
    CONST speeddata lowSpeed:= v20;
    CONST speeddata mediumSpeed:= v50;
    CONST speeddata fastSpeed:= v100;
    CONST speeddata ultraFastSpeed:= V500;
    !speed dependencies
    CONST speeddata homeApproachSpeed:= ultraFastSpeed;
    CONST speeddata workObjApproachSpeed:= ultraFastSpeed;
    CONST speeddata penPositioningSpeed:= mediumSpeed;
    CONST speeddata penLongPositioningSpeed:= ultraFastSpeed;
    CONST speeddata pendrawSpeed:= fastSpeed;
    
    ! MODEL DATA
    CONST num modelOutputWidthMM:= 200;
    CONST num letterHeight:= 200/scaleFactor;
    CONST num paddingWidth:= 20/scaleFactor;
    CONST num paddingWidthRight:= 400/scaleFactor-(letterMWidth + letterDWidth + letterXWidth + paddingWidth);
    CONST num paddingHeight:= 30/scaleFactor;
    CONST num outterRadius:= 100/scaleFactor;
    CONST num originalFrameWidth:= 400;
    ! dependencies
    CONST num scaleFactor:= originalFrameWidth/frameWidth;
    CONST num letterDWidth:= outterRadius;
    CONST num innerRadius:= outterRadius - paddingWidth;
    CONST num frameHeight:= letterHeight + paddingHeight*2;
   
    !letters
    !M
    CONST num letterMWidth:= 132.03/scaleFactor;
    CONST num mh1:= 65.28/scaleFactor;
    CONST num mh2:= 34.72/scaleFactor;
    CONST num letterXWidth:= 130/scaleFactor;
    !X
    CONST num xl := 22.47/scaleFactor;
    CONST num xh1:= 80.33/scaleFactor;
    CONST num xl1:= 51.22/scaleFactor;
       
    !=============
    !=== SETUP ===
    !=============
    
    ! rows and columns
    VAR bool anotherColumn:= false;
    VAR bool anotherRow:= false;
    VAR num rowCount:= 0;
    VAR num columnCount:= 0;
    
    CONST num frameWidth:= 400;
    VAR num margin:= 5;
    
    !=================================================
    !================= PROGRAM LOGIC =================
    !=================================================
    
    
    ! ========== MAIN PROCEDURE ==========
    PROC main()
        pen:= CTool();
        goHome;
        goToWorkObject;
        goToModelStart; 
        drawOnBoard;
        goHome;
    ENDPROC
    
    
    ! ========== HIGH-LEVEL PROCEDURES ==========
    
    ! draw MDX board
    PROC drawMDX() 
        drawFrame;
        drawLetterM;
        drawLetterD;
        drawLetterX;
    ENDPROC
    
    ! check for space on column and row to initate drawing procedure 
    PROC drawOnBoard()
        
        ! is another column available?
        IF (woMarginWidth+(columnCount+1)*(frameWidth + margin) < workObjWidth) THEN
            anotherColumn:=true;
        ENDIF
        
        ! if there is another column available
        WHILE(anotherColumn) DO
        
            ! if there is another row available
            IF (woMarginHeight+(rowCount+1)*(frameHeight + margin) < workObjHeight) THEN
                anotherRow:= true;
            ENDIF
            WHILE(anotherRow) DO
                drawMDX;
                rowCount:= rowCount + 1;
                IF (woMarginHeight+(rowCount + 1)*(frameHeight + margin) < workObjHeight) THEN
                    ! move up to next row
                    MoveL Offs(frameRef, frameHeight + margin, 0, penHoverHeight), pendrawSpeed,  fine, tool0; 
                ELSE
                    anotherRow:= false; 
                ENDIF
            ENDWHILE
            
            columnCount:= columnCount + 1;
            IF (woMarginWidth+margin+(columnCount+1)*(frameWidth + margin) < workObjWidth) THEN
                ! move right&down to a new column
                anotherColumn:= true;
                MoveL Offs(frameRef, -((rowCount-1)*(frameHeight+margin)), -frameWidth - margin, penHoverHeight), pendrawSpeed,  fine, tool0;
                rowCount:= 0;  
            ELSE
                anotherColumn:= false;
            ENDIF
                
        ENDWHILE
    
    ENDPROC
    
    
    
    ! ========== GENERAL ROBOT MOVEMENT PROCEDURES ==========
    
    !move into home position
	PROC goHome()
		MoveJ homePos, homeApproachSpeed, z200, tool0;
	ENDPROC
    
    !move to work object corner
	PROC goToWorkObject()
		MoveJ woOrgPos, workObjApproachSpeed, z200, tool0;
	ENDPROC

    !move at start of model
    PROC goToModelStart()
		MoveJ modelStartPos, penPositioningSpeed, fine, tool0;
	ENDPROC
    
    !move pen down/up
    ! - used at start and end of element drawing
    ! - used between letters/letters components
	PROC goPenDown()
        penPos:= CRobT(\Tool:=pen\WObj:=wobj0);
        MoveL Offs(penPos, 0, 0, -penHoverHeight), penPositioningSpeed, fine, tool0;
		
	ENDPROC
	PROC goPenUp()
		penPos:= CRobT(\Tool:=pen\WObj:=wobj0);
        MoveL Offs(penPos, 0, 0, penHoverHeight), penPositioningSpeed, fine, tool0;
	ENDPROC
    
    
    
    ! ========== DRAWING PROCEDURES ==========
    
    ! draw model frame
    PROC drawFrame()   
        goPenDown;
        frameRef:= CRobT(\Tool:=pen\WObj:=wobj0);
            MoveL Offs(frameRef, frameHeight, 0, 0), pendrawSpeed,  fine, pen;
            MoveL Offs(frameRef, frameHeight, -frameWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(frameRef, 0, -frameWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(frameRef, 0, 0, 0), pendrawSpeed, fine, pen;
        goPenUp;    
    ENDPROC
    
    !letters library
    !M
    PROC drawLetterM() 
        MoveL Offs(frameRef, paddingHeight, -paddingWidth, penHoverHeight), penLongPositioningSpeed,  fine, tool0;
        goPenDown;
        startElement:= CRobT(\Tool:=pen\WObj:=wobj0);
            MoveL Offs(startElement, letterHeight, 0, 0), pendrawSpeed,  fine, pen;
            MoveL Offs(startElement, letterHeight, -paddingWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight - mh1, -letterMWidth/2, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight, -letterMWidth + paddingWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight, -letterMWidth + paddingWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight, -letterMWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, -letterMWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, -letterMWidth + paddingWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight - mh2, -letterMWidth + paddingWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight - mh1 - mh2, -letterMWidth/2, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight - mh2, -paddingWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, -paddingWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, 0, 0), pendrawSpeed, fine, pen;
        goPenUp;  
    ENDPROC
   
    !D
    PROC drawLetterD()
        MoveL Offs(frameRef, paddingHeight, -(letterMWidth + paddingWidth*2) , penHoverHeight), penLongPositioningSpeed,  fine, tool0;
        goPenDown;
        startElement:= CRobT(\Tool:=pen\WObj:=wobj0);
            MoveC Offs(startElement, letterHeight/2, -outterRadius, 0), Offs(startElement, letterHeight, 0, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, 0, 0), pendrawSpeed, fine, pen;
            goPenUp;
            
            MoveL Offs(startElement, paddingWidth, -paddingWidth, penHoverHeight), pendrawSpeed, fine, tool0;
            goPenDown;
            startElement:= CRobT(\Tool:=pen\WObj:=wobj0);
                MoveC Offs(startElement, (letterHeight - paddingWidth/2)/2, -(innerRadius-paddingWidth), 0), Offs(startElement, letterHeight - paddingWidth*2, 0, 0), pendrawSpeed, fine, pen;
                MoveL Offs(startElement, 0, 0, 0), pendrawSpeed, fine, pen;
        
        goPenUp;  
    ENDPROC
    
    !X
    PROC drawLetterX()
         MoveL Offs(frameRef, paddingHeight, -(frameWidth - letterXWidth - paddingWidthRight), penHoverHeight), penLongPositioningSpeed,  fine, tool0;
         goPenDown;
         startElement:= CRobT(\Tool:=pen\WObj:=wobj0);
            MoveL Offs(startElement, letterHeight/2, -xl1, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight, 0, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight, -xl, 0), pendrawSpeed, fine, pen;
            
            MoveL Offs(startElement, letterHeight-xh1, -letterXWidth/2, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight, -(letterXWidth - xl), 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, letterHeight, -letterXWidth, 0), pendrawSpeed, fine, pen;
            
            MoveL Offs(startElement, letterHeight/2, -(letterXWidth-xl1), 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, -letterXWidth, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, -(letterXWidth - xl), 0), pendrawSpeed, fine, pen;
            
            MoveL Offs(startElement, xh1, -letterXWidth/2, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, -xl, 0), pendrawSpeed, fine, pen;
            MoveL Offs(startElement, 0, 0, 0), pendrawSpeed, fine, pen;
         goPenUp;  
    ENDPROC
    
ENDMODULE