import pymel.core as pm


'''Skeleton Fixes'''
#Freeze Skeleton 

pm.select('Collar_Jnt','Shoulder1','L_IK_Shoulder','L_IK_Elbow ','L_IK_Wrist','L_Hand','L_Finger1 ','L_Finger2 ','L_thumb1','L_thumb2','L_thumb3')
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1,jo=True)
pm.select('Shoulder2','R_IK_Shoulder','R_IK_Elbow ','R_IK_Wrist','R_Hand','R_Finger1 ','R_Finger2 ','R_thumb1','R_thumb2','R_thumb3')
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1,jo=True)
'''Leg Construction'''



#Left Leg Joint Reorient

pm.joint('L_Hip_Jnt',edit=True,oj='zyx')
pm.joint('L_Knee_Jnt',edit=True,oj='zyx')
pm.joint('L_Ankle_Jnt',edit=True,oj='zyx')
pm.joint('L_Ball_Jnt',edit=True,oj='zyx')
pm.joint('L_Toe_Jnt',edit=True,oj='none')

pm.joint('L_Hip_Jnt', edit=True, setPreferredAngles=True, ch=True)

#Right  LEgJoint Reorient

pm.joint('R_Hip_Jnt',edit=True,oj='zyx')
pm.joint('R_Knee_Jnt',edit=True,oj='zyx')
pm.joint('R_Ankle_Jnt',edit=True,oj='zyx')
pm.joint('R_Ball_Jnt',edit=True,oj='zyx')
pm.joint('R_Toe_Jnt',edit=True,oj='none')

pm.joint('R_Hip_Jnt', edit=True, setPreferredAngles=True, ch=True)

#sping reorient
pm.joint('Collar_Jnt',edit=True,oj='zyx')
#Left Arm Joint Reorient

pm.joint('Shoulder1',edit=True,oj='zyx')
pm.joint('L_IK_Elbow',edit=True,oj='zyx')
pm.joint('L_IK_Wrist',edit=True,oj='zyx')
pm.joint('L_Hand',edit=True,oj='zyx')


pm.joint('Shoulder1', edit=True, setPreferredAngles=True, ch=True)




#Right ArmJoint Reorient
               
pm.joint('Shoulder2',edit=True,oj='zyx')
pm.joint('R_IK_Elbow',edit=True,oj='zyx')
pm.joint('R_IK_Wrist',edit=True,oj='zyx')
pm.joint('R_Hand',edit=True,oj='zyx')


pm.joint('Shoulder2', edit=True, setPreferredAngles=True, ch=True)



#LeftIKhandle Create
pm.ikHandle(n='L_Leg',sj='L_Hip_Jnt', ee='L_Ankle_Jnt',sol='ikRPsolver')
pm.ikHandle(n='L_Ankle to Ball',sj='L_Ankle_Jnt',ee='L_Ball_Jnt')
pm.ikSolver( st='ikSCsolver',ep=0.5, n='SingleChainRight')
pm.ikHandle(n='L_Ball to Toe',sj='L_Ball_Jnt',ee='L_Toe_Jnt')
pm.select('L_Ankle_Ctrl_Jnt',r=True)
pm.select('L_Leg',add=True)
pm.pointConstraint('L_Ankle_Ctrl_Jnt', 'L_Leg')
pm.parent('L_Ankle_to_Ball','L_Ball_Ctrl_Jnt')
pm.parent('L_Ball_to_Toe','L_Toe_Ctrl_Jnt')


#RightIKHandle Create
pm.ikHandle(n='R_Leg',sj='R_Hip_Jnt',ee='R_Ankle_Jnt',sol='ikRPsolver')
pm.ikHandle(n='R_Ankle to Ball',sj='R_Ankle_Jnt',ee='R_Ball_Jnt')
pm.ikSolver( st='ikSCsolver',ep=0.5, n='SingleChain')
pm.ikHandle(n='R_Ball to Toe',sj='R_Ball_Jnt',ee='R_Toe_Jnt')
pm.select('R_Ankle_Ctrl_Jnt',r=True)
pm.select('R_Leg',add=True)
pm.pointConstraint('R_Ankle_Ctrl_Jnt', 'R_Leg')
pm.parent('R_Ankle_to_Ball','R_Ball_Ctrl_Jnt')
pm.parent('R_Ball_to_Toe','R_Toe_Ctrl_Jnt')


#Left Foot Controler Create

pm.circle(n='L_Foot_Ctrl',nr=(0,1,0),c=(0,0,0),tol='0.01')
Left_Foot_Ctrl_Pos = pm.xform('L_Ball_Jnt',query=True,ws=True,matrix=True)
pm.xform('L_Foot_Ctrl',ws=True,m=Left_Foot_Ctrl_Pos)
pm.duplicate('L_Foot_Ctrl',n='R_Foot_Ctrl')

Right_Foot_Ctrl_Pos = pm.xform('R_Ball_Jnt',query=True,ws=True,matrix=True)
pm.xform('R_Foot_Ctrl',ws=True,m=Right_Foot_Ctrl_Pos)

#Foot Controller Shape Creation 
    
pm.select('L_Foot_Ctrl','R_Foot_Ctrl')
pm.select('L_Foot_Ctrl.cv[0]','L_Foot_Ctrl.cv[1]','L_Foot_Ctrl.cv[2]','R_Foot_Ctrl.cv[0]','R_Foot_Ctrl.cv[1]','R_Foot_Ctrl.cv[2]')
pm.move(0, 0, -2.691589,r=True)
pm.select('L_Foot_Ctrl.cv[4]','L_Foot_Ctrl.cv[5]','L_Foot_Ctrl.cv[6]','R_Foot_Ctrl.cv[4]','R_Foot_Ctrl.cv[5]','R_Foot_Ctrl.cv[6]')
pm.move(0, 0 ,0.724986,r=True)
    
#Tie Foot to controller


pm.group('L_Heel_Ctrl_Jnt',n='L_Heel_Grp')
pm.group('R_Heel_Ctrl_Jnt',n='R_Heel_Grp')


#Tie Controller to the RHeel
pm.select('R_Foot_Ctrl',r=True)
pm.select('R_Heel_Grp',add=True)
pm.parentConstraint(mo=True, w=1)


#Tie Controller to the LHeel
pm.select('L_Foot_Ctrl',r=True)
pm.select('L_Heel_Grp',add=True)
pm.parentConstraint(mo=True, w=1)

#Add Attribute Values Left Heel Pivot
pm.select('L_Foot_Ctrl',r=True)
pm.addAttr(ln='heel_pivot',at='double',min=-10,max=10,dv=0)
pm.setAttr('L_Foot_Ctrl.heel_pivot',keyable=True)
pm.select('L_Foot_Ctrl',r=True)
pm.setDrivenKeyframe('L_Heel_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.heel_pivot')

pm.setAttr('L_Foot_Ctrl.heel_pivot',10,keyable=True)
pm.setAttr('L_Heel_Ctrl_Jnt.rotateZ',60,keyable=True)
pm.setDrivenKeyframe('L_Heel_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.heel_pivot')
pm.setAttr('L_Foot_Ctrl.heel_pivot',-10)
pm.setAttr('L_Heel_Ctrl_Jnt.rotateZ',-60)
pm.setDrivenKeyframe('L_Heel_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.heel_pivot')
pm.setAttr('L_Foot_Ctrl.heel_pivot',0)

#Creating Additional Attributes Left Foot
pm.addAttr(ln='heel_spin',at='double',min=-10,max=10,dv=0)
pm.setAttr('L_Foot_Ctrl.heel_spin',keyable=True)
pm.addAttr(ln='toe_pivot',at='double',min=-10,max=10,dv=0)
pm.setAttr('L_Foot_Ctrl.toe_pivot',keyable=True)
pm.addAttr(ln='ball_pivot',at='double',min=-10,max=10,dv=0)
pm.setAttr('L_Foot_Ctrl.ball_pivot',keyable=True)

#Add Attribute  Values Left Heel Spin 
pm.select('L_Heel_Ctrl_Jnt',r=True)
pm.setDrivenKeyframe('L_Heel_Ctrl_Jnt.rotateY',cd='L_Foot_Ctrl.heel_spin')
pm.setAttr('L_Foot_Ctrl.heel_spin',10)
pm.setAttr('L_Heel_Ctrl_Jnt.rotateY',60)
pm.setDrivenKeyframe('L_Heel_Ctrl_Jnt.rotateY',cd='L_Foot_Ctrl.heel_spin')
pm.setAttr('L_Foot_Ctrl.heel_spin',-10)
pm.setAttr('L_Heel_Ctrl_Jnt.rotateY',-60)
pm.setDrivenKeyframe('L_Heel_Ctrl_Jnt.rotateY',cd='L_Foot_Ctrl.heel_spin')
pm.setAttr('L_Foot_Ctrl.heel_spin',0)

#Add Attribute Values Left Toe Pivot 
pm.setDrivenKeyframe('L_Toe_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.toe_pivot')
pm.setAttr('L_Foot_Ctrl.toe_pivot',10)
pm.setAttr('L_Toe_Ctrl_Jnt.rotateZ',60)
pm.setDrivenKeyframe('L_Toe_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.toe_pivot')
pm.setAttr('L_Foot_Ctrl.toe_pivot',-10)
pm.setAttr('L_Toe_Ctrl_Jnt.rotateZ',-60)
pm.setDrivenKeyframe('L_Toe_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.toe_pivot')
pm.setAttr('L_Foot_Ctrl.toe_pivot',0)

#add Attribute Values Left Ball Pivot
pm.setDrivenKeyframe('L_Ball_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.ball_pivot')
pm.setAttr('L_Foot_Ctrl.ball_pivot',-10)
pm.setAttr('L_Ball_Ctrl_Jnt.rotateZ',-60)
pm.setDrivenKeyframe('L_Ball_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.ball_pivot')
pm.setAttr('L_Foot_Ctrl.ball_pivot',10)
pm.setAttr('L_Ball_Ctrl_Jnt.rotateZ',60)
pm.setDrivenKeyframe('L_Ball_Ctrl_Jnt.rotateZ',cd='L_Foot_Ctrl.ball_pivot')
pm.setAttr('L_Foot_Ctrl.ball_pivot',0)

#create Pole Vector Controller

pm.curve(n='Left_Knee_Controller',d=1,p=[(0 ,-0.798312, 4.287838),(0, 0.798312 ,3.580731),(-0.707107 ,-0.798312 ,3.580731), (0 , -0.798312  ,2.873624),(0, 0.798312 ,3.580731), (0.707107, -0.798312 ,3.580731) ,(0 ,-0.798312 ,4.287838),(-0.707107, -0.798312, 3.580731),( 0, -0.798312, 2.873624),(0.707107 ,-0.798312, 3.580731)],k=[0,1,2,3,4,5,6,7,8,9]) 
pm.xform(r=True,cp=True)
pm.setAttr('Left_Knee_Controller.rotateX',90)
pm.move(1.242267, -5.730831, -0.297442,rpr=True)
pm.duplicate('Left_Knee_Controller',n='Right_Knee_Controller')
pm.select('Right_Knee_Controller',r=True)
pm.move(-1.211395, -5.816898 ,-0.460511,rpr=True)
pm.select('Left_Knee_Controller','Right_Knee_Controller')
pm.move(0, 0, 7.844711,r=True)
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1)
pm.select('Left_Knee_Controller','L_Leg')
pm.poleVectorConstraint(w=1)
pm.select('Right_Knee_Controller','R_Leg')
pm.poleVectorConstraint(w=1)

#Add Attribute Values Right Heel Pivot

pm.select('R_Foot_Ctrl',r=True)
pm.addAttr(ln='heel_pivot',at='double',min=-10,max=10,dv=0)
pm.setAttr('R_Foot_Ctrl.heel_pivot',keyable=True)
pm.select('R_Foot_Ctrl',r=True)
pm.setDrivenKeyframe('R_Heel_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.heel_pivot')

pm.setAttr('R_Foot_Ctrl.heel_pivot',10,keyable=True)
pm.setAttr('R_Heel_Ctrl_Jnt.rotateZ',60,keyable=True)
pm.setDrivenKeyframe('R_Heel_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.heel_pivot')
pm.setAttr('R_Foot_Ctrl.heel_pivot',-10)
pm.setAttr('R_Heel_Ctrl_Jnt.rotateZ',-60)
pm.setDrivenKeyframe('R_Heel_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.heel_pivot')
pm.setAttr('R_Foot_Ctrl.heel_pivot',0)

#Creating Additional Attributes Right Foot
pm.addAttr(ln='heel_spin',at='double',min=-10,max=10,dv=0)
pm.setAttr('R_Foot_Ctrl.heel_spin',keyable=True)
pm.addAttr(ln='toe_pivot',at='double',min=-10,max=10,dv=0)
pm.setAttr('R_Foot_Ctrl.toe_pivot',keyable=True)
pm.addAttr(ln='ball_pivot',at='double',min=-10,max=10,dv=0)
pm.setAttr('R_Foot_Ctrl.ball_pivot',keyable=True)

#add Attribute  Values Right Heel Spin 
pm.select('R_Heel_Ctrl_Jnt',r=True)
pm.setDrivenKeyframe('R_Heel_Ctrl_Jnt.rotateY',cd='R_Foot_Ctrl.heel_spin')
pm.setAttr('R_Foot_Ctrl.heel_spin',10)
pm.setAttr('R_Heel_Ctrl_Jnt.rotateY',60)
pm.setDrivenKeyframe('R_Heel_Ctrl_Jnt.rotateY',cd='R_Foot_Ctrl.heel_spin')
pm.setAttr('R_Foot_Ctrl.heel_spin',-10)
pm.setAttr('R_Heel_Ctrl_Jnt.rotateY',-60)
pm.setDrivenKeyframe('R_Heel_Ctrl_Jnt.rotateY',cd='R_Foot_Ctrl.heel_spin')
pm.setAttr('R_Foot_Ctrl.heel_spin',0)

#add Attribute Values Right Toe Pivot 
pm.setDrivenKeyframe('R_Toe_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.toe_pivot')
pm.setAttr('R_Foot_Ctrl.toe_pivot',10)
pm.setAttr('R_Toe_Ctrl_Jnt.rotateZ',60)
pm.setDrivenKeyframe('R_Toe_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.toe_pivot')
pm.setAttr('R_Foot_Ctrl.toe_pivot',-10)
pm.setAttr('R_Toe_Ctrl_Jnt.rotateZ',-60)
pm.setDrivenKeyframe('R_Toe_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.toe_pivot')
pm.setAttr('R_Foot_Ctrl.toe_pivot',0)


#add Attribute Values Left Ball Pivot
pm.setDrivenKeyframe('R_Ball_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.ball_pivot')
pm.setAttr('R_Foot_Ctrl.ball_pivot',-10)
pm.setAttr('R_Ball_Ctrl_Jnt.rotateZ',-60)
pm.setDrivenKeyframe('R_Ball_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.ball_pivot')
pm.setAttr('R_Foot_Ctrl.ball_pivot',10)
pm.setAttr('R_Ball_Ctrl_Jnt.rotateZ',60)
pm.setDrivenKeyframe('R_Ball_Ctrl_Jnt.rotateZ',cd='R_Foot_Ctrl.ball_pivot')
pm.setAttr('R_Foot_Ctrl.ball_pivot',0)

#Freeze Transformations on Foot

pm.select('L_Foot_Ctrl','R_Foot_Ctrl')
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1)



''' Mid Body Contruction '''

#Pelvis Controller

pm.circle( n='Center_Controller',nr=(0, 1, 0), c=(0, 0, 0) , sw=360,r=1,d=3,ut=False,tol=0.01,s=24,cch=True)
pm.scale(4.0,4.0,4.0,r=True)
pm.select('Center_Controller.cv[1]','Center_Controller.cv[3]','Center_Controller.cv[5]','Center_Controller.cv[7]','Center_Controller.cv[9]','Center_Controller.cv[11]','Center_Controller.cv[13]','Center_Controller.cv[15]','Center_Controller.cv[17]','Center_Controller.cv[19]','Center_Controller.cv[21]','Center_Controller.cv[23]')
pm.scale(.3,.3,.3,pivot=(0,0,0),r=True)
pm.select('Center_Controller')
pm.scale(2.5,2.5,2.5,r=True)
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1)

#Back Controllers Create

pm.curve(n='Bottom_Center_Controller',d=3,p=[(0, 0, 0),(0 ,0 ,-6),(0, -0.499797, -7.37584),(0,  -1.30294 ,-6.319076),(0, -0.837963 ,-5.727286 ),(0 ,-0.457527 ,-5.980911),(0 ,-0.542068 ,-6.445888 ),(0, -0.795692 ,-6.488159),(0 ,-0.880233 ,-6.319076)],k=[0,0,0,1,2,3,4,5,6,6,6])
pm.scale(1, 1, 1.235647,r=True)

pm.group('Bottom_Center_Controller','Bottom_Center_Controller',n='Bottom_Center_Ctrl_Grp',r=True)
pm.select('Bottom_Center_Controller.cv[0:8]')
pm.rotate(0,-90,0,r=True)
pm.rotate(0,0,-90,r=True)

# Creating Back Controller and Mid Controller

#Creating Center Controller link 

#Creating Bottom Center Back Controllers
pm.parentConstraint('Lumbar_Jnt','Bottom_Center_Ctrl_Grp',mo=False,w=1)
pm.select('Bottom_Center_Controller')
Bottom_Center_Ctrl_Pos = pm.xform('Lumbar_Jnt',query=True,ws=True,matrix=True)
pm.xform('Bottom_Center_Controller',ws=True,m=Bottom_Center_Ctrl_Pos)
Bottom_Center_Pivot_Pos = pm.xform('Lumbar_Jnt',query=True,ws=True,matrix=True)
pm.xform('Bottom_Center_Ctrl_Grp_parentConstraint1.scalePivot', 'Bottom_Center_Ctrl_Grp_parentConstraint1.rotatePivot',ws=True,m=Bottom_Center_Pivot_Pos)
pm.delete('Bottom_Center_Ctrl_Grp_parentConstraint1')


pm.duplicate('Bottom_Center_Ctrl_Grp',n='Mid_Center_Ctrl_Grp')
pm.duplicate('Bottom_Center_Ctrl_Grp',n='Top_Center_Ctrl_Grp')
pm.duplicate('Bottom_Center_Ctrl_Grp',n='Head_Center_Ctrl_Grp')
pm.duplicate('Bottom_Center_Ctrl_Grp',n='Left_Center_Ctrl_Grp')
pm.duplicate('Bottom_Center_Ctrl_Grp',n='Right_Center_Ctrl_Grp')
pm.rename('Mid_Center_Ctrl_Grp|Bottom_Center_Controller','Mid_Center_Controller')
pm.rename('Top_Center_Ctrl_Grp|Bottom_Center_Controller','Top_Center_Controller')
pm.rename('Head_Center_Ctrl_Grp|Bottom_Center_Controller','Head_Center_Controller')
pm.rename('Left_Center_Ctrl_Grp|Bottom_Center_Controller','Left_Center_Controller')
pm.rename('Right_Center_Ctrl_Grp|Bottom_Center_Controller','Right_Center_Controller')

#Positioning Mid Center Controller
Mid_Center_Ctrl_Grp_Pos = pm.xform('Thorax_Jnt',query=True,ws=True,matrix=True)
pm.xform('Mid_Center_Ctrl_Grp',ws=True,m=Mid_Center_Ctrl_Grp_Pos)
Mid_Center_Ctrl_Pos = pm.xform('Thorax_Jnt',query=True,ws=True,matrix=True)
pm.xform('Mid_Center_Controller',ws=True,m=Mid_Center_Ctrl_Pos)
pm.parentConstraint('Mid_Center_Controller','Thorax_Jnt',mo=True,w=1)
pm.delete('Thorax_Jnt_parentConstraint1')

#Positioning Top Center Controller 
Top_Center_Ctrl_Grp_Pos = pm.xform('Neck_Jnt',query=True,ws=True,matrix=True)
pm.xform('Top_Center_Ctrl_Grp',ws=True,m=Top_Center_Ctrl_Grp_Pos)
Top_Center_Ctrl_Pos = pm.xform('Neck_Jnt',query=True,ws=True,matrix=True)
pm.xform('Top_Center_Controller',ws=True,m=Top_Center_Ctrl_Pos)
pm.parentConstraint('Top_Center_Controller','Neck_Jnt',mo=True,w=1)
pm.delete('Neck_Jnt_parentConstraint1')

#Position Head Center Controller
Head_Center_Ctrl_Grp_Pos = pm.xform('Head_Jnt',query=True,ws=True,matrix=True)
pm.xform('Head_Center_Ctrl_Grp',ws=True,m=Head_Center_Ctrl_Grp_Pos)
Head_Center_Ctrl_Pos = pm.xform('Head_Jnt',query=True,ws=True,matrix=True)
pm.xform('Head_Center_Controller',ws=True,m=Head_Center_Ctrl_Pos)
pm.parentConstraint('Head_Center_Controller','Head_Jnt',mo=True,w=1)
pm.delete('Head_Jnt_parentConstraint1')

#Position Left Center Controller
Left_Center_Ctrl_Grp_Pos = pm.xform('Shoulder1',query=True,ws=True,matrix=True)
pm.xform('Left_Center_Ctrl_Grp',ws=True,m=Left_Center_Ctrl_Grp_Pos)
Left_Center_Ctrl_Pos = pm.xform('Shoulder1',query=True,ws=True,matrix=True)
pm.xform('Left_Center_Controller',ws=True,m=Left_Center_Ctrl_Pos)
pm.parentConstraint('Left_Center_Controller','Shoulder1',mo=True,w=1)
pm.delete('Shoulder1_parentConstraint1')

#Position Right Center Controller
Right_Center_Ctrl_Grp_Pos = pm.xform('Shoulder2',query=True,ws=True,matrix=True)
pm.xform('Right_Center_Ctrl_Grp',ws=True,m=Right_Center_Ctrl_Grp_Pos)
Right_Center_Ctrl_Pos = pm.xform('Shoulder2',query=True,ws=True,matrix=True)
pm.xform('Right_Center_Controller',ws=True,m=Right_Center_Ctrl_Pos)
pm.parentConstraint('Right_Center_Controller','Shoulder2',mo=True,w=1)
pm.delete('Shoulder2_parentConstraint1')

#reorient Top Center Controller
pm.select('Top_Center_Controller.cv[0:8]')
pm.rotate(0,0,0,r=True)

#reorient Left Center Controller
pm.select('Left_Center_Controller.cv[0:8]')
pm.rotate(90,0,90,r=True)
#Reorient Right Center COntroller 
pm.select('Right_Center_Controller.cv[0:8]')
pm.rotate(-90,0,-90,r=True)


#Freeze Transformations on all Back Controllers 
pm.select('Bottom_Center_Controller','Mid_Center_Controller','Top_Center_Controller','Head_Center_Controller','Left_Center_Controller','Right_Center_Controller')
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1)


pm.parentConstraint('Center_Controller','Head_Ctrl1',mo=True,w=1)
pm.delete('Head_Ctrl1_parentConstraint1')

#Make Hip And Back Controllers Great Again!!!

pm.parentConstraint('Center_Controller','Head_Ctrl1',mo=True,w=1)
pm.parentConstraint('Center_Controller','Bottom_Center_Ctrl_Grp',mo=True,w=1)
pm.parentConstraint('Bottom_Center_Controller','Mid_Center_Ctrl_Grp',mo=True,w=1)
pm.spaceLocator(p=(0,0,0),n='Neck_Shift_Locator')
Head_Locator = pm.xform('Head_Jnt',query=True,ws=True,matrix=True)
pm.xform('Neck_Shift_Locator',ws=True,m=Head_Locator)
pm.parentConstraint('Mid_Center_Controller','Neck_Shift_Locator',mo=True,w=1)
pm.pointConstraint('Neck_Shift_Locator','Top_Center_Controller',mo=True,w=1)
pm.parentConstraint('Top_Center_Controller','Head_Center_Ctrl_Grp',mo=True,w=1)
pm.parentConstraint('Bottom_Center_Controller','Lumbar_Jnt',mo=True,w=1)
pm.parentConstraint('Mid_Center_Controller','Thorax_Jnt',mo=True,w=1)
pm.parentConstraint('Top_Center_Controller','Neck_Jnt',mo=True,w=1)
pm.parentConstraint('Head_Center_Controller','Head_Jnt',mo=True,w=1)

#Place left and right Shoulder Controllers
pm.parentConstraint('Shoulder1','Left_Center_Ctrl_Grp',w=1)
pm.parentConstraint('Shoulder2','Right_Center_Ctrl_Grp',w=1)
pm.select('Left_Center_Controller.cv[0:8]')
pm.rotate(135,90,0,r=True)
pm.select('Right_Center_Controller.cv[0:8]')
pm.rotate(-135,-90,0,r=True)
pm.select('Left_Center_Ctrl_Grp','Right_Center_Ctrl_Grp')
pm.scale(.75,1,1,r=True)

pm.delete('Right_Center_Ctrl_Grp_parentConstraint1')
pm.delete('Left_Center_Ctrl_Grp_parentConstraint1')
pm.parentConstraint('Right_Center_Controller','Shoulder2',mo=True,w=1)
pm.parentConstraint('Left_Center_Controller','Shoulder1',mo=True,w=1)
pm.parentConstraint('Bottom_Center_Controller','Right_Center_Controller',mo=True,w=1)
pm.parentConstraint('Bottom_Center_Controller','Left_Center_Controller',mo=True,w=1)
pm.parentConstraint('Mid_Center_Controller','Right_Center_Controller',mo=True,w=1)
pm.parentConstraint('Mid_Center_Controller','Left_Center_Controller',mo=True,w=1)
pm.parentConstraint('Top_Center_Controller','Head_Center_Controller',mo=True,w=1)
pm.parentConstraint('Center_Controller','Bottom_Center_Controller',mo=True,w=1)

#Create Ik Arm Handles

pm.ikHandle(n='L_Arm',sj='L_IK_Shoulder',ee='L_IK_Wrist',sol='ikRPsolver')
pm.ikHandle(n='R_Arm',sj='R_IK_Shoulder',ee='R_IK_Wrist',sol='ikRPsolver')

#Create,Position and Attach Elbow Controller
pm.select('Left_Knee_Controller')

#pm.duplicate('Left__Controller',n='Left_Elbow_Controller')
pm.curve(n='Left_Elbow_Controller',d=1,p=[(0 ,-0.798312, 4.287838),(0, 0.798312 ,3.580731),(-0.707107 ,-0.798312 ,3.580731), (0 , -0.798312  ,2.873624),(0, 0.798312 ,3.580731), (0.707107, -0.798312 ,3.580731) ,(0 ,-0.798312 ,4.287838),(-0.707107, -0.798312, 3.580731),( 0, -0.798312, 2.873624),(0.707107 ,-0.798312, 3.580731)],k=[0,1,2,3,4,5,6,7,8,9])
pm.curve(n='Right_Elbow_Controller',d=1,p=[(0 ,-0.798312, 4.287838),(0, 0.798312 ,3.580731),(-0.707107 ,-0.798312 ,3.580731), (0 , -0.798312  ,2.873624),(0, 0.798312 ,3.580731), (0.707107, -0.798312 ,3.580731) ,(0 ,-0.798312 ,4.287838),(-0.707107, -0.798312, 3.580731),( 0, -0.798312, 2.873624),(0.707107 ,-0.798312, 3.580731)],k=[0,1,2,3,4,5,6,7,8,9])
#Freeze Transformations
pm.select('Left_Elbow_Controller','Right_Elbow_Controller')
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1)




#Move Elbow Ctrl Vector to The RIght Place

pm.spaceLocator(p=(0,0,0),n='Left_Elbow_Shift_Locator')
L_Elbow_Locator = pm.xform('L_IK_Elbow',query=True,ws=True,matrix=True)
pm.xform('Left_Elbow_Shift_Locator',ws=True,m=L_Elbow_Locator)

pm.spaceLocator(p=(0,0,0),n='Right_Elbow_Shift_Locator')
R_Elbow_Locator = pm.xform('R_IK_Elbow',query=True,ws=True,matrix=True)
pm.xform('Right_Elbow_Shift_Locator',ws=True,m=R_Elbow_Locator)


Left_Elbow_Ctrl_Pos = pm.xform('Left_Elbow_Shift_Locator',query=True,ws=True,matrix=True)
pm.xform('Left_Elbow_Controller',ws=True,m=Left_Elbow_Ctrl_Pos)

Right_Elbow_Ctrl_Pos = pm.xform('Right_Elbow_Shift_Locator',query=True,ws=True,matrix=True)
pm.xform('Right_Elbow_Controller',ws=True,m=Right_Elbow_Ctrl_Pos)



pm.scale(-1.5,1,1,r=True)



#pm.select('Left_Elbow_Controller','Right_Elbow_Controller'
pm.select('Left_Elbow_Controller','Right_Elbow_Controller')
pm.move(0,0,-9,r=True)


#Constrain Left Elbow Pole Vector to the controller
pm.select('Left_Elbow_Controller','L_Arm')
pm.poleVectorConstraint(w=1)
#Constrain Left Elbow Pole Vector to the controller
pm.select('Right_Elbow_Controller','R_Arm')
pm.poleVectorConstraint(w=1)

#Create Hand Controller

pm.circle(n='L_Hand_Ctrl',nr=(0,1,0),c=(0,0,0),tol='0.01')

Left_Hand_Ctrl_Pos = pm.xform('L_Hand',query=True,ws=True,matrix=True)
pm.xform('L_Hand_Ctrl',ws=True,m=Left_Hand_Ctrl_Pos)
Left_Hand_Pivot_Pos = pm.xform('L_IK_Wrist',query=True,ws=True,matrix=True)
pm.xform('L_Hand_Ctrl.scalePivot', 'L_Hand_Ctrl.rotatePivot',ws=True,m=Left_Hand_Pivot_Pos)


pm.duplicate('L_Hand_Ctrl',n='R_Hand_Ctrl')


Right_Hand_Ctrl_Pos = pm.xform('R_Hand',query=True,ws=True,matrix=True)
pm.xform('R_Hand_Ctrl',ws=True,m=Right_Hand_Ctrl_Pos)
Right_Hand_Pivot_Pos = pm.xform('R_IK_Wrist',query=True,ws=True,matrix=True)
pm.xform('R_Hand_Ctrl.scalePivot', 'R_Hand_Ctrl.rotatePivot',ws=True,m=Right_Hand_Pivot_Pos)


pm.select('L_Hand_Ctrl','R_Hand_Ctrl')
pm.scale(2.5,2.5,1,r=True)


#Contraining arm controllers to the Wrist IK handle 

pm.parentConstraint('L_Hand_Ctrl','L_Arm',mo=True,w=1)
pm.parentConstraint('R_Hand_Ctrl','R_Arm',mo=True,w=1)

#Contraining The Arms To the Body
pm.parentConstraint('Center_Controller','Left_Center_Ctrl_Grp',mo=True,w=1)
pm.parentConstraint('Center_Controller','Right_Center_Ctrl_Grp',mo=True,w=1)

#Rotate Hand Controllers
pm.select('L_Hand_Ctrl.cv[0:8]')
pm.rotate(-50,0,0,r=True)
pm.select('R_Hand_Ctrl.cv[0:8]')
pm.rotate(50,0,0,r=True)
#Move Elbow Controllers

pm.select('Right_Elbow_Controller.cv[0:9]')
pm.move(3.5,0,0,r=True)
pm.select('Left_Elbow_Controller.cv[0:9]')
pm.move(-3.5,0,0,r=True)


#Freeze Transformations of Recently added Controllers
pm.select('R_Hand_Ctrl','L_Hand_Ctrl','Left_Elbow_Controller')
pm.makeIdentity(apply=True,t=1,r=1,s=1,n=0,pn=1)


#Set up Hand Controller
pm.group('L_Hand_Ctrl','L_Hand_Ctrl',n='L_Hand_Ctrl_Grp')
pm.group('R_Hand_Ctrl','R_Hand_Ctrl',n='R_Hand_Ctrl_Grp')
pm.select('R_Hand_Ctrl','R_IK_Wrist')
pm.orientConstraint(mo=True,w=1)
pm.select('L_Hand_Ctrl','L_IK_Wrist')
pm.orientConstraint(mo=True,w=1)
#pm.delete('L_IK_Wrist_orientConstraint1')
'''Lock Controllers '''

#Section Meant to make controllers move only where they are suppose too 

#BottomCenter Controller Lock
pm.setAttr('Bottom_Center_Controller.tx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Bottom_Center_Controller.ty', lock=True, keyable=False, channelBox=False)
pm.setAttr('Bottom_Center_Controller.tz', lock=True, keyable=False, channelBox=False)
pm.setAttr('Bottom_Center_Controller.sx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Bottom_Center_Controller.sy', lock=True, keyable=False, channelBox=False)
pm.setAttr('Bottom_Center_Controller.sz', lock=True, keyable=False, channelBox=False)

#MidCenter Controller Lock
pm.setAttr('Mid_Center_Controller.tx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Mid_Center_Controller.ty', lock=True, keyable=False, channelBox=False)
pm.setAttr('Mid_Center_Controller.tz', lock=True, keyable=False, channelBox=False)
pm.setAttr('Mid_Center_Controller.sx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Mid_Center_Controller.sy', lock=True, keyable=False, channelBox=False)
pm.setAttr('Mid_Center_Controller.sz', lock=True, keyable=False, channelBox=False)

#RightCenter Controller Lock

pm.setAttr('Right_Center_Controller.sx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Right_Center_Controller.sy', lock=True, keyable=False, channelBox=False)
pm.setAttr('Right_Center_Controller.sz', lock=True, keyable=False, channelBox=False)

#LeftCenter Controller Lock

pm.setAttr('Left_Center_Controller.sx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Left_Center_Controller.sy', lock=True, keyable=False, channelBox=False)
pm.setAttr('Left_Center_Controller.sz', lock=True, keyable=False, channelBox=False)

#TopCenter Controller Lock
pm.setAttr('Top_Center_Controller.tx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Top_Center_Controller.ty', lock=True, keyable=False, channelBox=False)
pm.setAttr('Top_Center_Controller.tz', lock=True, keyable=False, channelBox=False)
pm.setAttr('Top_Center_Controller.sx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Top_Center_Controller.sy', lock=True, keyable=False, channelBox=False)
pm.setAttr('Top_Center_Controller.sz', lock=True, keyable=False, channelBox=False)

#HeadCenter Controller Lock
pm.setAttr('Head_Center_Controller.tx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Head_Center_Controller.ty', lock=True, keyable=False, channelBox=False)
pm.setAttr('Head_Center_Controller.tz', lock=True, keyable=False, channelBox=False)
pm.setAttr('Head_Center_Controller.sx', lock=True, keyable=False, channelBox=False)
pm.setAttr('Head_Center_Controller.sy', lock=True, keyable=False, channelBox=False)
pm.setAttr('Head_Center_Controller.sz', lock=True, keyable=False, channelBox=False)

