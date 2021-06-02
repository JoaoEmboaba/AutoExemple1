# AutoExemple1
Projeto usado como exemplo para a navegação do robô da FRC, tendo um loop autônomo e comandos para a conexão do joystick


#include <frc/Joystick.h> // Biblioteca para os controles.
#include <frc/PWMSparkMax.h> // Controla os motores(Victors).
#include <frc/TimedRobot.h> // Biblioteca para teste.
#include <frc/Timer.h> // Usado para o circuito autônomo.
#include <frc/drive/DifferentialDrive.h> //   Conecta o controle nos Victors.
#include <frc/livewindow/LiveWindow.h>
#include <iostream> //  bah, tão cansados de saber né?
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
    cout << "TechMakerRobotics" << endl;
    if (m_timer.Get() <= 3.0) {             
    m_robotDrive.ArcadeDrive(0.45, 0.0);
      else if (m_timer.Get() <= 4.0) {
      m_robotDrive.ArcadeDrive(0.0, 0.35);
      else if (m_timer.Get() <= 5.0) {
      m_robotDrive.ArcadeDrive(0.45, 0.0);
      else if (m_timer.Get() <= 6.0) {
      m_robotDrive.ArcadeDrive(0.0, -0.35);
      else if (m_timer.Get() <= 7.0) {
      m_robotDrive.ArcadeDrive(0.45, 0.0);
      else if (m_time.Get() <= 8.0) {
        m_robotDrive.ArcadeDrive(0.0, 0.35);
        else if (m_time.Get() <= 9.0) {
          m_robotDrive.ArcadeDrive(0.45, 0.0);
          else if (m_timer.Get() <= 10.0) {
            m_robotDrive.ArcadeDrive(0.0, -0.35);
            else if (m_timer.Get() <= 11.0) {
              m_robotDrive.ArcadeDrive (0.45, 0.0);
              else if (m_timer.Get() <= 12.0) {
                m_robotDrive.ArcadeDrive (0.45, 0.55);
              }
            }
          }
        }
       }
      
      return 1;
    }
 }    
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
