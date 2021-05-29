// # AutoExemple1
// Projeto usado como exemplo para a navegação do robô da FRC, tendo um loop autônomo e comandos para a conexão do joystick



#include <frc/Joystick.h> //Library for the controls
#include <frc/PWMSparkMax.h> //Control the motors
#include <frc/TimedRobot.h> //Library to the test
#include <frc/Timer.h> //Used to Autonomous circuit
#include <frc/XboxController.h> // Connect the joystick on the Victor
#include <frc/livewindow/LiveWindow.h>
#include "frc/GenericHID.h"
namespace frc {
#include <iostream> // bah, we're all tired of know
using namespace std;


class XboxController : public : GenericHID {
 public:
  
  explicit XboxControlle(int port);  

  ~XboxController(XboxController&&) = default;

  XboxController & operator=(XboxController&&) = default;

   float GetX(JoystickHand hand) const override;
  
   float GetY(JoystickHand hand) const override;
  
   float GetTriggerAxis(JoystickHand hand) const;
  
   bool GetBumper(JoystickHand hand) const;
  
   bool GetBumperPressed(JoystickHand hand);
  
   bool GetBumperReleased(JoystickHand hand);
  
   bool GetStickButton(JoystickHand hand) const;
  
   bool GetStickButtonPressed(JoystickHand hand);
  
   bool GetStickButtonReleased(JoystickHand hand);
  
   bool GetAButton() const;
  
   bool GetAButtonPressed();
  
   bool GetAButtonReleased();
  
   bool GetBButton() const;
  
   bool GetBButtonPressed();
  
   bool GetBButtonReleased();
  
   bool GetXButton() const;
  
   bool GetXButtonPressed();
  
   bool GetXButtonReleased();
  
   bool GetYButton() const;
  
   bool GetYButtonPressed();
  
   bool GetYButtonReleased();
  
   bool GetBackButton() const;
  
   bool GetBackButtonPressed();
  
   bool GetBackButtonReleased();
  
   bool GetStartButton() const;
  
   bool GetStartButtonPressed();
  
   bool GetStartButtonReleased();

   enum class Button {
     kBumperLeft = 5,
     kBumperRight = 6,
     kStickLeft = 9,
     kStickRight = 10,
     kA = 1,
     kB = 2,
     kX = 3,
     kY = 4,
     kBack = 7,
     kStart = 8
   };
  
   enum class Axis {
     kLeftX = 0,
     kRightX = 4,
     kLeftY = 1,
     kRightY = 5,
     kLeftTrigger = 2,
     kRightTrigger = 3
   };
 };
  
  void AutonomousInit() override {
    m_timer.Reset();
    m_timer.Start();
  }

  void AutonomousPeriodic() override {
    // Drive for 2 seconds
    cout << "TechMakerRobotics\n" << endl;
    if (m_timer.Get() <= 5.0) {      
      // Drive forwards half speed
      m_robotDrive.ArcadeDrive(0.5, 0.0);
    } else {
      // Stop robot
      m_robotDrive.ArcadeDrive(-0.5, 0.0);
      delay(0.3, 0.0);
      m_robotDrive.ArcadeDrive(-0.3, 0.0);
      m_robotDrive.ArcadeDrive(0.0, 0.0);   
    }

  }

  void TeleopInit() override {}

  void TeleopPeriodic() override {

  }

  void TestInit() override {}

  void TestPeriodic() override {}

 private:

  frc::LiveWindow& m_lw = *frc::LiveWindow::GetInstance();
  frc::Timer m_timer;
};

#ifndef RUNNING_FRC_TESTS
int main() {
  return frc::StartRobot<Robot>();
}
#endif
