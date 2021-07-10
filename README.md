// Copyright (c) FIRST and other WPILib contributors.
// Open Source Software; you can modify and/or share it under the terms of
// the WPILib BSD license file in the root directory of this project.

#include <frc/XboxController.h>
#include <frc/PWMVictorSPX.h>
#include <frc/TimedRobot.h>
#include <frc/Timer.h>
#include <frc/drive/DifferentialDrive.h>
#include <frc/livewindow/LiveWindow.h>
#include <frc2/command/SubsystemBase.h>
#include "ctre/phoenix.h"
#include <frc/SpeedControllerGroup.h>

class Robot : public frc::TimedRobot {
  
  public:
  
  double rAccel = 0, lAccel = 0;
  Robot()
  {
    m_timer.Start();
  }

  void AutonomousInit() override
  {
    m_timer.Reset();
    m_timer.Start();
  }

  void AutonomousPeriodic() override
  {
    if (m_timer.Get() < 6.0)
    {
      m_robotDrive.ArcadeDrive(-0.45, 0.0 );
      m_shooterLeft.Set(1.0);
      m_shooterRight.Set(1.0);
      m_shooterMiddle.Set(1.0);
    }else if (m_timer.Get () < 7.0) {
      m_robotDrive.ArcadeDrive(0.0, 0.75); 
    } else if (m_timer.Get () < 13.0) {
      m_robotDrive.ArcadeDrive (-0.45, 0.0);
    }
     else{
      m_robotDrive.ArcadeDrive(0.0, 0.0);
      m_shooterLeft.Set(0);
      m_shooterRight.Set(0);
      m_shooterMiddle.Set(0);
    }
  }

  void TeleopInit() override {
    lAccel = 0;
    rAccel = 0;
  }

  void TeleopPeriodic() override
  {
      lAccel = m_stick.GetTriggerAxis(lHand);
      m_shooterMiddle.Set(lAccel);
      rAccel = m_stick.GetTriggerAxis(rHand);
      m_shooterLeft.Set(rAccel);
      m_shooterRight.Set(rAccel);
        m_robotDrive.ArcadeDrive(
        m_driverController.GetY(frc::GenericHID::JoystickHand::kLeftHand),
        m_driverController.GetX(frc::GenericHID::JoystickHand::kRightHand));
  }
      

  void TestInit() override {}

  void TestPeriodic() override {}

private:
  // Robot drive system
  WPI_VictorSPX m_frontLeft{4};
  WPI_VictorSPX m_frontRight{5};
  WPI_VictorSPX m_rearLeft{6};
  WPI_VictorSPX m_rearRight{7};
  WPI_VictorSPX m_shooterRight{3};
  WPI_VictorSPX m_shooterLeft{2};
  WPI_VictorSPX m_shooterMiddle{1};

  frc::SpeedControllerGroup m_left{m_frontLeft, m_rearLeft};
  frc::SpeedControllerGroup m_right{m_frontRight, m_rearRight};
  frc::DifferentialDrive m_robotDrive{m_right,m_left};

  WPI_VictorSPX m_leftMotor{1};
  WPI_VictorSPX m_rightMotor{2};
  frc::XboxController m_driverController{0};
  frc::GenericHID::JoystickHand lHand = frc::GenericHID::kLeftHand;
  frc::GenericHID::JoystickHand rHand = frc::GenericHID::kRightHand;

  frc::XboxController m_stick{0};
  frc::LiveWindow &m_lw = *frc::LiveWindow::GetInstance();
  frc::Timer m_timer;
};

#ifndef RUNNING_FRC_TESTS
int main()
{
  return frc::StartRobot<Robot>();
}
#endif
