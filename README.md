# AutoExemple1
Projeto usado como exemplo para a navegação do robô da FRC, tendo um loop autônomo e comandos para a conexão do joystick



#include <frc/Joystick.h> //Library for the controls
#include <frc/PWMSparkMax.h> //Control the motors
#include <frc/TimedRobot.h> //Library to the test
#include <frc/Timer.h> //Used to Autonomous circuit
#include <frc/drive/DifferentialDrive.h> // Conect the joystick on the Victor
#include <frc/livewindow/LiveWindow.h>
#include <iostream> // bah, we're all tired of know
using namespace std;
class Robot : public frc::TimedRobot {
 public:
  Robot() {
    m_robotDrive.SetExpiration(0.1);
    m_timer.Start();
  }

  void AutonomousInit() override {
    m_timer.Reset();
    m_timer.Start();
  }

  void AutonomousPeriodic() override {
    // Drive for 2 seconds
    cout << "TechMakerRobotics" << endl;
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
    // Drive with arcade style (use right stick)
    m_robotDrive.ArcadeDrive(m_stick.GetY(), m_stick.GetX());
  }

  void TestInit() override {}

  void TestPeriodic() override {}

 private:
  // Robot drive system
  frc::PWMSparkMax m_left{0};
  frc::PWMSparkMax m_right{1};
  frc::DifferentialDrive m_robotDrive{m_left, m_right};

  frc::Joystick m_stick{0};
  frc::LiveWindow& m_lw = *frc::LiveWindow::GetInstance();
  frc::Timer m_timer;
};

#ifndef RUNNING_FRC_TESTS
int main() {
  return frc::StartRobot<Robot>();
}
#endif
